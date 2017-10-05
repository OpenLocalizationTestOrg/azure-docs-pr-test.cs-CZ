---
title: "Systému SQL Server FCI – virtuální počítače Azure | Microsoft Docs"
description: "Tento článek vysvětluje, jak vytvořit Instance clusteru převzetí služeb při selhání SQL serveru na virtuálních počítačích Azure."
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
ms.openlocfilehash: 439353b7d22fb7376049ea8e1433a8d5840d3e0f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a><span data-ttu-id="0e361-103">Konfigurace Instance clusteru převzetí služeb při selhání systému SQL Server na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="0e361-103">Configure SQL Server Failover Cluster Instance on Azure Virtual Machines</span></span>

<span data-ttu-id="0e361-104">Tento článek vysvětluje, jak vytvořit SQL Server převzetí služeb při selhání clusteru Instance (FCI) na virtuálních počítačích Azure v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0e361-104">This article explains how to create a SQL Server Failover Cluster Instance (FCI) on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="0e361-105">Toto řešení používá [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) jako softwarová virtuální síť SAN, synchronizuje úložiště (datových disků) mezi uzly (virtuální počítače Azure) v clusteru se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="0e361-105">This solution uses [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) as a software-based virtual SAN that synchronizes the storage (data disks) between the nodes (Azure VMs) in a Windows Cluster.</span></span> <span data-ttu-id="0e361-106">S2D je nového v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="0e361-106">S2D is new in Windows Server 2016.</span></span>

<span data-ttu-id="0e361-107">Následující diagram znázorňuje kompletního řešení na virtuálních počítačích Azure:</span><span class="sxs-lookup"><span data-stu-id="0e361-107">The following diagram shows the complete solution on Azure virtual machines:</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

<span data-ttu-id="0e361-109">Na předchozím obrázku uvádí:</span><span class="sxs-lookup"><span data-stu-id="0e361-109">The preceding diagram shows:</span></span>

- <span data-ttu-id="0e361-110">Dva virtuální počítače Azure v clusteru s podporou převzetí služeb při selhání systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0e361-110">Two Azure virtual machines in a Windows Failover Cluster.</span></span> <span data-ttu-id="0e361-111">Pokud virtuální počítač v clusteru s podporou převzetí služeb při selhání se také označuje jako *uzlu clusteru*, nebo *uzly*.</span><span class="sxs-lookup"><span data-stu-id="0e361-111">When a virtual machine is in a failover cluster it is also called a *cluster node*, or *nodes*.</span></span>
- <span data-ttu-id="0e361-112">Každý virtuální počítač má dva nebo více datových disků.</span><span class="sxs-lookup"><span data-stu-id="0e361-112">Each virtual machine has two or more data disks.</span></span>
- <span data-ttu-id="0e361-113">S2D synchronizuje data na datový disk a uvede synchronizované úložiště jako fond úložiště.</span><span class="sxs-lookup"><span data-stu-id="0e361-113">S2D synchronizes the data on the data disk and presents the synchronized storage as a storage pool.</span></span>
- <span data-ttu-id="0e361-114">Fond úložiště představuje sdílený svazek clusteru (CSV) do clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-114">The storage pool presents a cluster shared volume (CSV) to the failover cluster.</span></span>
- <span data-ttu-id="0e361-115">Role clusteru SQL serveru FCI používá sdílený svazek clusteru pro datové jednotky.</span><span class="sxs-lookup"><span data-stu-id="0e361-115">The SQL Server FCI cluster role uses the CSV for the data drives.</span></span>
- <span data-ttu-id="0e361-116">K Azure pro vyrovnávání zatížení pro uložení IP adresu pro SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-116">An Azure load balancer to hold the IP address for the SQL Server FCI.</span></span>
- <span data-ttu-id="0e361-117">Nastavení Azure dostupnosti obsahuje všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e361-117">An Azure availability set holds all the resources.</span></span>

   >[!NOTE]
   ><span data-ttu-id="0e361-118">Všechny prostředky Azure jsou v diagramu jsou ve stejné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e361-118">All Azure resources are in the diagram are in the same resource group.</span></span>

<span data-ttu-id="0e361-119">Podrobnosti o S2D najdete v tématu [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span><span class="sxs-lookup"><span data-stu-id="0e361-119">For details about S2D, see [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span></span>

<span data-ttu-id="0e361-120">S2D podporuje dva typy architektury - sblížené a hyperkonvergované.</span><span class="sxs-lookup"><span data-stu-id="0e361-120">S2D supports two types of architectures - converged and hyper-converged.</span></span> <span data-ttu-id="0e361-121">Architektura v tomto dokumentu je hyperkonvergované.</span><span class="sxs-lookup"><span data-stu-id="0e361-121">The architecture in this document is hyper-converged.</span></span> <span data-ttu-id="0e361-122">V infrastruktuře hyperkonvergované umístí úložiště na stejné servery, které jsou hostiteli clusterové aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e361-122">A hyper-converged infrastructure places the storage on the same servers that host the clustered application.</span></span> <span data-ttu-id="0e361-123">V této architektuře úložiště je na každém uzlu SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-123">In this architecture, the storage is on each SQL Server FCI node.</span></span>

### <a name="example-azure-template"></a><span data-ttu-id="0e361-124">Příklad šablony Azure</span><span class="sxs-lookup"><span data-stu-id="0e361-124">Example Azure template</span></span>

<span data-ttu-id="0e361-125">Celé řešení v Azure můžete vytvořit ze šablony.</span><span class="sxs-lookup"><span data-stu-id="0e361-125">You can create the entire solution in Azure from a template.</span></span> <span data-ttu-id="0e361-126">Příklad šablony je k dispozici v Githubu [šablon Azure rychlý Start](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span><span class="sxs-lookup"><span data-stu-id="0e361-126">An example of a template is available in the GitHub [Azure Quickstart Templates](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span></span> <span data-ttu-id="0e361-127">V tomto příkladu není určená nebo testována pro všechny konkrétní úlohu.</span><span class="sxs-lookup"><span data-stu-id="0e361-127">This example is not designed or tested for any specific workload.</span></span> <span data-ttu-id="0e361-128">Když spustíte šablonu, kterou chcete vytvořit SQL Server FCI s S2D úložiště připojené k vaší doméně.</span><span class="sxs-lookup"><span data-stu-id="0e361-128">You can run the template to create a SQL Server FCI with S2D storage connected to your domain.</span></span> <span data-ttu-id="0e361-129">Můžete vyhodnotit šablony a upravit pro vaše záměry.</span><span class="sxs-lookup"><span data-stu-id="0e361-129">You can evaluate the template, and modify it for your purposes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0e361-130">Než začnete</span><span class="sxs-lookup"><span data-stu-id="0e361-130">Before you begin</span></span>

<span data-ttu-id="0e361-131">Existuje několik věcí, které je nutné znát a několik věcí, které budete potřebovat na místě před můžete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="0e361-131">There are a few things you need to know and a couple of things that you need in place before you proceed.</span></span>

### <a name="what-to-know"></a><span data-ttu-id="0e361-132">Co potřebujete vědět</span><span class="sxs-lookup"><span data-stu-id="0e361-132">What to know</span></span>
<span data-ttu-id="0e361-133">Musí mít provozní znalosti o technologiích následující:</span><span class="sxs-lookup"><span data-stu-id="0e361-133">You should have an operational understanding of the following technologies:</span></span>

- [<span data-ttu-id="0e361-134">Technologie clusteru systému Windows</span><span class="sxs-lookup"><span data-stu-id="0e361-134">Windows cluster technologies</span></span>](http://technet.microsoft.com/library/hh831579.aspx)
-  <span data-ttu-id="0e361-135">[Instance clusteru převzetí služeb při selhání SQL serveru](http://msdn.microsoft.com/library/ms189134.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e361-135">[SQL Server Failover Cluster Instances](http://msdn.microsoft.com/library/ms189134.aspx).</span></span>

<span data-ttu-id="0e361-136">Kromě toho musí mít obecné znalosti následujících technologií:</span><span class="sxs-lookup"><span data-stu-id="0e361-136">Also, you should have a general understanding of the following technologies:</span></span>

- [<span data-ttu-id="0e361-137">Konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="0e361-137">Hyper-converged solution using Storage Spaces Direct in Windows Server 2016</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [<span data-ttu-id="0e361-138">Skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="0e361-138">Azure resource groups</span></span>](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-to-have"></a><span data-ttu-id="0e361-139">Co je potřeba mít</span><span class="sxs-lookup"><span data-stu-id="0e361-139">What to have</span></span>

<span data-ttu-id="0e361-140">Než budete postupovat podle pokynů v tomto článku, byste již měli mít:</span><span class="sxs-lookup"><span data-stu-id="0e361-140">Before following the instructions in this article, you should already have:</span></span>

- <span data-ttu-id="0e361-141">Předplatné Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="0e361-141">A Microsoft Azure subscription.</span></span>
- <span data-ttu-id="0e361-142">Domény systému Windows na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="0e361-142">A Windows domain on Azure virtual machines.</span></span>
- <span data-ttu-id="0e361-143">Účet s oprávněním k vytváření objektů ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="0e361-143">An account with permission to create objects in the Azure virtual machine.</span></span>
- <span data-ttu-id="0e361-144">Virtuální síti Azure a podsítě s IP Adresou dostatečný Adresní prostor pro následující součásti:</span><span class="sxs-lookup"><span data-stu-id="0e361-144">An Azure virtual network and subnet with sufficient IP address space for the following components:</span></span>
   - <span data-ttu-id="0e361-145">Virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-145">Both virtual machines.</span></span>
   - <span data-ttu-id="0e361-146">IP adresa clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-146">The failover cluster IP address.</span></span>
   - <span data-ttu-id="0e361-147">IP adresa pro každý FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-147">An IP address for each FCI.</span></span>
- <span data-ttu-id="0e361-148">DNS nakonfigurovaný v síti Azure, odkazující na řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="0e361-148">DNS configured on the Azure Network, pointing to the domain controllers.</span></span>

<span data-ttu-id="0e361-149">Tyto požadavky splněny můžete pokračovat s vytvářením clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-149">With these prerequisites in place, you can proceed with building your failover cluster.</span></span> <span data-ttu-id="0e361-150">Prvním krokem je vytvoření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e361-150">The first step is to create the virtual machines.</span></span>

## <a name="step-1-create-virtual-machines"></a><span data-ttu-id="0e361-151">Krok 1: Vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="0e361-151">Step 1: Create virtual machines</span></span>

1. <span data-ttu-id="0e361-152">Přihlaste se k [portál Azure](http://portal.azure.com) s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="0e361-152">Log in to the [Azure portal](http://portal.azure.com) with your subscription.</span></span>

1. <span data-ttu-id="0e361-153">[Vytvořit skupinu dostupnosti Azure](../tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="0e361-153">[Create an Azure availability set](../tutorial-availability-sets.md).</span></span>

   <span data-ttu-id="0e361-154">Dostupnost v rámci domén selhání nastavit skupiny virtuálních počítačů a aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="0e361-154">The availability set groups virtual machines across fault domains and update domains.</span></span> <span data-ttu-id="0e361-155">Skupiny dostupnosti je zajištěno, že vaše aplikace není ovlivněn jediný bod selhání, jako je síťový přepínač nebo jednotka power rack serverů.</span><span class="sxs-lookup"><span data-stu-id="0e361-155">The availability set makes sure that your application is not affected by single points of failure, like the network switch or the power unit of a rack of servers.</span></span>

   <span data-ttu-id="0e361-156">Pokud jste dosud nevytvořili skupinu prostředků pro virtuální počítače, když vytvoříte skupinu dostupnosti Azure ho proveďte.</span><span class="sxs-lookup"><span data-stu-id="0e361-156">If you have not created the resource group for your virtual machines, do it when you create an Azure availability set.</span></span> <span data-ttu-id="0e361-157">Pokud používáte portál Azure k vytvoření skupiny dostupnosti, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0e361-157">If you're using the Azure portal to create the availability set, do the following steps:</span></span>

   - <span data-ttu-id="0e361-158">Na portálu Azure klikněte na tlačítko  **+**  otevřete Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0e361-158">In the Azure portal, click **+** to open the Azure Marketplace.</span></span> <span data-ttu-id="0e361-159">Vyhledejte **sadu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="0e361-159">Search for **Availability set**.</span></span>
   - <span data-ttu-id="0e361-160">Klikněte na tlačítko **sadu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="0e361-160">Click **Availability set**.</span></span>
   - <span data-ttu-id="0e361-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0e361-161">Click **Create**.</span></span>
   - <span data-ttu-id="0e361-162">Na **vytvořit skupinu dostupnosti** okno, nastavte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="0e361-162">On the **Create availability set** blade, set the following values:</span></span>
      - <span data-ttu-id="0e361-163">**Název**: název sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="0e361-163">**Name**: A name for the availability set.</span></span>
      - <span data-ttu-id="0e361-164">**Předplatné**: Azure vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="0e361-164">**Subscription**: Your Azure subscription.</span></span>
      - <span data-ttu-id="0e361-165">**Skupina prostředků**: Pokud chcete použít existující skupinu, klikněte na tlačítko **použít existující** a vyberte skupinu z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0e361-165">**Resource group**: If you want to use an existing group, click **Use existing** and select the group from the drop-down list.</span></span> <span data-ttu-id="0e361-166">Jinak vyberte **vytvořit nový** a zadejte název pro skupinu.</span><span class="sxs-lookup"><span data-stu-id="0e361-166">Otherwise choose **Create New** and type a name for the group.</span></span>
      - <span data-ttu-id="0e361-167">**Umístění**: nastavte umístění, kde chcete vytvořit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-167">**Location**: Set the location where you plan to create your virtual machines.</span></span>
      - <span data-ttu-id="0e361-168">**Poruch domén**: použijte výchozí (3).</span><span class="sxs-lookup"><span data-stu-id="0e361-168">**Fault domains**: Use the default (3).</span></span>
      - <span data-ttu-id="0e361-169">**Aktualizovat domén**: použijte výchozí (5).</span><span class="sxs-lookup"><span data-stu-id="0e361-169">**Update domains**: Use the default (5).</span></span>
   - <span data-ttu-id="0e361-170">Klikněte na tlačítko **vytvořit** vytvoření dostupnost sady.</span><span class="sxs-lookup"><span data-stu-id="0e361-170">Click **Create** to create the availability set.</span></span>

1. <span data-ttu-id="0e361-171">Vytvoření virtuálních počítačů v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="0e361-171">Create the virtual machines in the availability set.</span></span>

   <span data-ttu-id="0e361-172">Zřídit dva virtuální počítače systému SQL Server v sadě Azure dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="0e361-172">Provision two SQL Server virtual machines in the Azure availability set.</span></span> <span data-ttu-id="0e361-173">Pokyny najdete v tématu [zřízení virtuálního počítače s SQL serverem na portálu Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="0e361-173">For instructions, see [Provision a SQL Server virtual machine in the Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

   <span data-ttu-id="0e361-174">Umístíte virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="0e361-174">Place both virtual machines:</span></span>

   - <span data-ttu-id="0e361-175">Ve službě Azure stejné skupině prostředků, kterou vaše dostupnosti je v.</span><span class="sxs-lookup"><span data-stu-id="0e361-175">In the same Azure resource group that your availability set is in.</span></span>
   - <span data-ttu-id="0e361-176">Ve stejné síti jako řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="0e361-176">On the same network as your domain controller.</span></span>
   - <span data-ttu-id="0e361-177">V podsíti s dostatkem místa IP adresu pro virtuální počítače a všech instancích Fci, které může nakonec použijete na tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-177">On a subnet with sufficient IP address space for both virtual machines, and all FCIs that you may eventually use on this cluster.</span></span>
   - <span data-ttu-id="0e361-178">V sadě Azure dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="0e361-178">In the Azure availability set.</span></span>   

      >[!IMPORTANT]
      ><span data-ttu-id="0e361-179">Nelze nastavit nebo změnit dostupnosti nastavit po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-179">You cannot set or change availability set after a virtual machine has been created.</span></span>

   <span data-ttu-id="0e361-180">Vyberte bitovou kopii z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0e361-180">Choose an image from the Azure Marketplace.</span></span> <span data-ttu-id="0e361-181">Můžete použít na trhu bitové kopie s, který zahrnuje Windows Server a SQL Server nebo jenom Windows Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-181">You can use a Marketplace image with that includes Windows Server and SQL Server, or just the Windows Server.</span></span> <span data-ttu-id="0e361-182">Podrobnosti najdete v tématu [přehled systému SQL Server na virtuálních počítačích Azure](../../virtual-machines-windows-sql-server-iaas-overview.md)</span><span class="sxs-lookup"><span data-stu-id="0e361-182">For details, see [Overview of SQL Server on Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)</span></span>

   <span data-ttu-id="0e361-183">Oficiální imagí SQL serveru v galerii Azure zahrnují nainstalovaná instance systému SQL Server, plus instalace softwaru SQL Server a vyžadovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="0e361-183">The official SQL Server images in the Azure Gallery include an installed SQL Server instance, plus the SQL Server installation software, and the required key.</span></span>

   <span data-ttu-id="0e361-184">Vyberte bitovou kopii správné podle způsob platit za licenci systému SQL Server:</span><span class="sxs-lookup"><span data-stu-id="0e361-184">Choose the right image according to how you want to pay for the SQL Server license:</span></span>

   - <span data-ttu-id="0e361-185">**Platba za použití licencování**: náklady za minutu těchto bitových kopií zahrnuje licencování SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="0e361-185">**Pay per usage licensing**: The per-minute cost of these images includes the SQL Server licensing:</span></span>
      - <span data-ttu-id="0e361-186">**SQL Server 2016 Enterprise na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="0e361-186">**SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="0e361-187">**SQL Server 2016 Standard na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="0e361-187">**SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="0e361-188">**SQL Server 2016 vývojáře v systému Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="0e361-188">**SQL Server 2016 Developer on Windows Server Datacenter 2016**</span></span>

   - <span data-ttu-id="0e361-189">**Převést – vaše – vlastní – licence (BYOL)**</span><span class="sxs-lookup"><span data-stu-id="0e361-189">**Bring-your-own-license (BYOL)**</span></span>

      - <span data-ttu-id="0e361-190">**{BYOL} SQL Server 2016 Enterprise na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="0e361-190">**{BYOL} SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="0e361-191">**{BYOL} SQL Server 2016 Standard na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="0e361-191">**{BYOL} SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>

   >[!IMPORTANT]
   ><span data-ttu-id="0e361-192">Po vytvoření virtuálního počítače odeberte předem nainstalovaná samostatnou instanci serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="0e361-192">After you create the virtual machine, remove the pre-installed standalone SQL Server instance.</span></span> <span data-ttu-id="0e361-193">Chcete-li vytvořit SQL Server FCI po konfiguraci clusteru převzetí služeb při selhání a S2D použijete předem nainstalovaná média systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-193">You will use the pre-installed SQL Server media to create the SQL Server FCI after you configure the failover cluster and S2D.</span></span>

   <span data-ttu-id="0e361-194">Alternativně můžete použít Image Azure Marketplace s pouze operační systém.</span><span class="sxs-lookup"><span data-stu-id="0e361-194">Alternatively, you can use Azure Marketplace images with just the operating system.</span></span> <span data-ttu-id="0e361-195">Vyberte **Windows Server 2016 Datacenter** bitovou kopii a nainstalujte SQL Server FCI po konfiguraci clusteru převzetí služeb při selhání a S2D.</span><span class="sxs-lookup"><span data-stu-id="0e361-195">Choose a **Windows Server 2016 Datacenter** image and install the SQL Server FCI after you configure the failover cluster and S2D.</span></span> <span data-ttu-id="0e361-196">Tento image neobsahuje instalačního média systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-196">This image does not contain SQL Server installation media.</span></span> <span data-ttu-id="0e361-197">Umístíte na instalačním médiu v umístění, kde můžete spouštět instalace systému SQL Server pro každý server.</span><span class="sxs-lookup"><span data-stu-id="0e361-197">Place the installation media in a location where you can run the SQL Server installation for each server.</span></span>

1. <span data-ttu-id="0e361-198">Jakmile Azure vytvoří virtuální počítače, připojení všem virtuálním počítačům pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="0e361-198">After Azure creates your virtual machines, connect to each virtual machine with RDP.</span></span>

   <span data-ttu-id="0e361-199">Když poprvé připojíte k virtuálnímu počítači pomocí protokolu RDP, počítač požádá, pokud chcete povolit tento počítač zjistitelné v síti.</span><span class="sxs-lookup"><span data-stu-id="0e361-199">When you first connect to a virtual machine with RDP, the computer asks if you want to allow this PC to be discoverable on the network.</span></span> <span data-ttu-id="0e361-200">Klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="0e361-200">Click **Yes**.</span></span>

1. <span data-ttu-id="0e361-201">Pokud použijete jednu z imagí virtuálního počítače se systémem SQL Server, odeberte instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-201">If you are using one of the SQL Server-based virtual machine images, remove the SQL Server instance.</span></span>

   - <span data-ttu-id="0e361-202">V **programy a funkce**, klikněte pravým tlačítkem na **Microsoft SQL Server 2016 (64 bitů)** a klikněte na tlačítko **odinstalovat nebo změnit**.</span><span class="sxs-lookup"><span data-stu-id="0e361-202">In **Programs and Features**, right-click **Microsoft SQL Server 2016 (64-bit)** and click **Uninstall/Change**.</span></span>
   - <span data-ttu-id="0e361-203">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="0e361-203">Click **Remove**.</span></span>
   - <span data-ttu-id="0e361-204">Vyberte výchozí instanci.</span><span class="sxs-lookup"><span data-stu-id="0e361-204">Select the default instance.</span></span>
   - <span data-ttu-id="0e361-205">Odeberte všechny funkce v části **služby databázového stroje**.</span><span class="sxs-lookup"><span data-stu-id="0e361-205">Remove all features under **Database Engine Services**.</span></span> <span data-ttu-id="0e361-206">Neodebírejte **sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="0e361-206">Do not remove **Shared Features**.</span></span> <span data-ttu-id="0e361-207">Viz následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="0e361-207">See the following picture:</span></span>

      ![Odebrat funkce](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - <span data-ttu-id="0e361-209">Klikněte na tlačítko **Další**a potom klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="0e361-209">Click **Next**, and then click **Remove**.</span></span>

1. <span data-ttu-id="0e361-210"><a name="ports"></a>Otevřete porty brány firewall.</span><span class="sxs-lookup"><span data-stu-id="0e361-210"><a name="ports"></a>Open the firewall ports.</span></span>

   <span data-ttu-id="0e361-211">Na každém virtuálním počítači otevřete následující porty v bráně Windows Firewall.</span><span class="sxs-lookup"><span data-stu-id="0e361-211">On each virtual machine, open the following ports on the Windows Firewall.</span></span>

   | <span data-ttu-id="0e361-212">Účel</span><span class="sxs-lookup"><span data-stu-id="0e361-212">Purpose</span></span> | <span data-ttu-id="0e361-213">TCP Port</span><span class="sxs-lookup"><span data-stu-id="0e361-213">TCP Port</span></span> | <span data-ttu-id="0e361-214">Poznámky</span><span class="sxs-lookup"><span data-stu-id="0e361-214">Notes</span></span>
   | ------ | ------ | ------
   | <span data-ttu-id="0e361-215">SQL Server</span><span class="sxs-lookup"><span data-stu-id="0e361-215">SQL Server</span></span> | <span data-ttu-id="0e361-216">1433</span><span class="sxs-lookup"><span data-stu-id="0e361-216">1433</span></span> | <span data-ttu-id="0e361-217">Normální port pro výchozí instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-217">Normal port for default instances of SQL Server.</span></span> <span data-ttu-id="0e361-218">Pokud jste použili bitovou kopii z galerie, se automaticky otevře tento port.</span><span class="sxs-lookup"><span data-stu-id="0e361-218">If you used an image from the gallery, this port is automatically opened.</span></span>
   | <span data-ttu-id="0e361-219">Test stavu</span><span class="sxs-lookup"><span data-stu-id="0e361-219">Health probe</span></span> | <span data-ttu-id="0e361-220">59999</span><span class="sxs-lookup"><span data-stu-id="0e361-220">59999</span></span> | <span data-ttu-id="0e361-221">Žádné otevřete TCP port.</span><span class="sxs-lookup"><span data-stu-id="0e361-221">Any open TCP port.</span></span> <span data-ttu-id="0e361-222">Později, nakonfigurujte pro vyrovnávání zatížení [test stavu](#probe) a cluster na tento port použít.</span><span class="sxs-lookup"><span data-stu-id="0e361-222">In a later step, configure the load balancer [health probe](#probe) and the cluster to use this port.</span></span>  

1. <span data-ttu-id="0e361-223">Přidání úložiště do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-223">Add storage to the virtual machine.</span></span> <span data-ttu-id="0e361-224">Podrobné informace najdete v tématu [přidejte úložiště](../../../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="0e361-224">For detailed information, see [add storage](../../../storage/common/storage-premium-storage.md).</span></span>

   <span data-ttu-id="0e361-225">Virtuální počítače nutné aspoň dva datových disků.</span><span class="sxs-lookup"><span data-stu-id="0e361-225">Both virtual machines need at least two data disks.</span></span>

   <span data-ttu-id="0e361-226">Připojte nezpracovaná disky - není NTFS naformátovaný disky.</span><span class="sxs-lookup"><span data-stu-id="0e361-226">Attach raw disks - not NTFS formatted disks.</span></span>
      >[!NOTE]
      ><span data-ttu-id="0e361-227">Pokud připojíte naformátovaném systémem souborů NTFS disků, můžete povolit jenom S2D s žádná kontrola způsobilosti disku.</span><span class="sxs-lookup"><span data-stu-id="0e361-227">If you attach NTFS-formatted disks, you can only enable S2D with no disk eligibility check.</span></span>  

   <span data-ttu-id="0e361-228">Pro každý virtuální počítač připojte minimálně dvě Storage úrovně Premium (SSD disky).</span><span class="sxs-lookup"><span data-stu-id="0e361-228">Attach a minimum of two Premium Storage (SSD disks) to each VM.</span></span> <span data-ttu-id="0e361-229">Doporučujeme alespoň P30 disky (1 TB).</span><span class="sxs-lookup"><span data-stu-id="0e361-229">We recommend at least P30 (1 TB) disks.</span></span>

   <span data-ttu-id="0e361-230">Sada hostitele ukládání do mezipaměti ke **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="0e361-230">Set host caching to **Read-only**.</span></span>

   <span data-ttu-id="0e361-231">Kapacita úložiště, které můžete použít v produkčním prostředí závisí na velikosti pracovní zátěže.</span><span class="sxs-lookup"><span data-stu-id="0e361-231">The storage capacity you use in production environments depends on your workload.</span></span> <span data-ttu-id="0e361-232">Hodnoty popsané v tomto článku jsou pro demonstrační a testování.</span><span class="sxs-lookup"><span data-stu-id="0e361-232">The values described in this article are for demonstration and testing.</span></span>

1. <span data-ttu-id="0e361-233">[Přidejte virtuální počítače k existující doméně](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="0e361-233">[Add the virtual machines to your pre-existing domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

<span data-ttu-id="0e361-234">Po virtuální počítače jsou vytvoření a konfiguraci, můžete nakonfigurovat cluster převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-234">After the virtual machines are created and configured, you can configure the failover cluster.</span></span>

## <a name="step-2-configure-the-windows-failover-cluster-with-s2d"></a><span data-ttu-id="0e361-235">Krok 2: Konfigurace clusteru převzetí služeb při selhání systému Windows s S2D</span><span class="sxs-lookup"><span data-stu-id="0e361-235">Step 2: Configure the Windows Failover Cluster with S2D</span></span>

<span data-ttu-id="0e361-236">Dalším krokem je konfigurace clusteru převzetí služeb při selhání s S2D.</span><span class="sxs-lookup"><span data-stu-id="0e361-236">The next step is to configure the failover cluster with S2D.</span></span> <span data-ttu-id="0e361-237">V tomto kroku provedete následující dílčích kroků:</span><span class="sxs-lookup"><span data-stu-id="0e361-237">In this step, you will do the following substeps:</span></span>

1. <span data-ttu-id="0e361-238">Přidejte funkci Clustering převzetí služeb při selhání systému Windows</span><span class="sxs-lookup"><span data-stu-id="0e361-238">Add Windows Failover Clustering feature</span></span>
1. <span data-ttu-id="0e361-239">Ověření clusteru</span><span class="sxs-lookup"><span data-stu-id="0e361-239">Validate the cluster</span></span>
1. <span data-ttu-id="0e361-240">Vytvoření clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="0e361-240">Create the failover cluster</span></span>
1. <span data-ttu-id="0e361-241">Vytvoření určujícího prvku cloudu</span><span class="sxs-lookup"><span data-stu-id="0e361-241">Create the cloud witness</span></span>
1. <span data-ttu-id="0e361-242">Přidání úložiště</span><span class="sxs-lookup"><span data-stu-id="0e361-242">Add storage</span></span>

### <a name="add-windows-failover-clustering-feature"></a><span data-ttu-id="0e361-243">Přidejte funkci Clustering převzetí služeb při selhání systému Windows</span><span class="sxs-lookup"><span data-stu-id="0e361-243">Add Windows Failover Clustering feature</span></span>

1. <span data-ttu-id="0e361-244">Pokud chcete začít, připojte k první virtuální počítač s RDP pomocí účtu domény, který je členem místní skupiny administrators a má oprávnění k vytváření objektů ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e361-244">To begin, connect to the first virtual machine with RDP using a domain account that is a member of local administrators, and has permissions to create objects in Active Directory.</span></span> <span data-ttu-id="0e361-245">Tento účet použijte pro ostatní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0e361-245">Use this account for the rest of the configuration.</span></span>

1. <span data-ttu-id="0e361-246">[Přidejte funkci Clustering převzetí služeb při selhání na každém virtuálním počítači](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="0e361-246">[Add Failover Clustering feature to each virtual machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

   <span data-ttu-id="0e361-247">Instalace funkce Clustering převzetí služeb při selhání z uživatelského rozhraní, proveďte následující kroky na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-247">To install Failover Clustering feature from the UI, do the following steps on both virtual machines.</span></span>
   - <span data-ttu-id="0e361-248">V **správce serveru**, klikněte na tlačítko **spravovat**a potom klikněte na **přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="0e361-248">In **Server Manager**, click **Manage**, and then click **Add Roles and Features**.</span></span>
   - <span data-ttu-id="0e361-249">V **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další** až na **vybrat funkce**.</span><span class="sxs-lookup"><span data-stu-id="0e361-249">In **Add Roles and Features Wizard**, click **Next** until you get to **Select Features**.</span></span>
   - <span data-ttu-id="0e361-250">V **vybrat funkce**, klikněte na tlačítko **Clustering převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="0e361-250">In **Select Features**, click **Failover Clustering**.</span></span> <span data-ttu-id="0e361-251">Zahrnout všechny požadované funkce a nástroje pro správu.</span><span class="sxs-lookup"><span data-stu-id="0e361-251">Include all required features and the management tools.</span></span> <span data-ttu-id="0e361-252">Klikněte na tlačítko **přidat funkce**.</span><span class="sxs-lookup"><span data-stu-id="0e361-252">Click **Add Features**.</span></span>
   - <span data-ttu-id="0e361-253">Klikněte na tlačítko **Další** a pak klikněte na **Dokončit** nainstalovat funkce.</span><span class="sxs-lookup"><span data-stu-id="0e361-253">Click **Next** and then click **Finish** to install the features.</span></span>

   <span data-ttu-id="0e361-254">K instalaci funkce Clustering převzetí služeb při selhání v prostředí PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e361-254">To install the Failover Clustering feature with PowerShell, run the following script from an administrator PowerShell session on one of the virtual machines.</span></span>

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

<span data-ttu-id="0e361-255">Pro referenci další kroky, postupujte podle pokynů v kroku 3 [konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="0e361-255">For reference, the next steps follow the instructions under Step 3 of [Hyper-converged solution using Storage Spaces Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span></span>

### <a name="validate-the-cluster"></a><span data-ttu-id="0e361-256">Ověření clusteru</span><span class="sxs-lookup"><span data-stu-id="0e361-256">Validate the cluster</span></span>

<span data-ttu-id="0e361-257">Tato příručka označuje pokyny v části [ověření clusteru](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span><span class="sxs-lookup"><span data-stu-id="0e361-257">This guide refers to instructions under [validate cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span></span>

<span data-ttu-id="0e361-258">Ověření clusteru v uživatelském rozhraní nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0e361-258">Validate the cluster in the UI or with PowerShell.</span></span>

<span data-ttu-id="0e361-259">Pro ověření clusteru s uživatelským rozhraním, proveďte následující kroky z jednoho z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e361-259">To validate the cluster with the UI, do the following steps from one of the virtual machines.</span></span>

1. <span data-ttu-id="0e361-260">V **správce serveru**, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="0e361-260">In **Server Manager**, click **Tools**, then click **Failover Cluster Manager**.</span></span>
1. <span data-ttu-id="0e361-261">V **Správce clusteru převzetí služeb při selhání**, klikněte na tlačítko **akce**, pak klikněte na tlačítko **ověřit konfiguraci...** .</span><span class="sxs-lookup"><span data-stu-id="0e361-261">In **Failover Cluster Manager**, click **Action**, then click **Validate Configuration...**.</span></span>
1. <span data-ttu-id="0e361-262">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0e361-262">Click **Next**.</span></span>
1. <span data-ttu-id="0e361-263">Na **vybrat servery nebo Cluster**, zadejte název virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-263">On **Select Servers or a Cluster**, type the name of both virtual machines.</span></span>
1. <span data-ttu-id="0e361-264">Na **testování možnosti**, zvolte **spustit jenom testy, vyberte**.</span><span class="sxs-lookup"><span data-stu-id="0e361-264">On **Testing options**, choose **Run only tests I select**.</span></span> <span data-ttu-id="0e361-265">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0e361-265">Click **Next**.</span></span>
1. <span data-ttu-id="0e361-266">Na **testování výběr**, zahrnout všechny testy s výjimkou **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="0e361-266">On **Test selection**, include all tests except **Storage**.</span></span> <span data-ttu-id="0e361-267">Viz následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="0e361-267">See the following picture:</span></span>

   ![Ověřit testy](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. <span data-ttu-id="0e361-269">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="0e361-269">Click **Next**.</span></span>
1. <span data-ttu-id="0e361-270">Na **potvrzení**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="0e361-270">On **Confirmation**, click **Next**.</span></span>

<span data-ttu-id="0e361-271">**Průvodce ověřením konfigurace** spustí testy pro ověření.</span><span class="sxs-lookup"><span data-stu-id="0e361-271">The **Validate a Configuration Wizard** runs the validation tests.</span></span>

<span data-ttu-id="0e361-272">Pro ověření clusteru v prostředí PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="0e361-272">To validate the cluster with PowerShell, run the following script from an administrator PowerShell session on one of the virtual machines.</span></span>

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

<span data-ttu-id="0e361-273">Po ověření clusteru, vytvořte cluster převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-273">After you validate the cluster, create the failover cluster.</span></span>

### <a name="create-the-failover-cluster"></a><span data-ttu-id="0e361-274">Vytvoření clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="0e361-274">Create the failover cluster</span></span>

<span data-ttu-id="0e361-275">Tato příručka označuje [vytvořit cluster převzetí služeb při selhání](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span><span class="sxs-lookup"><span data-stu-id="0e361-275">This guide refers to [Create the failover cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span></span>

<span data-ttu-id="0e361-276">Pokud chcete vytvořit cluster převzetí služeb při selhání, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="0e361-276">To create the failover cluster, you need:</span></span>
- <span data-ttu-id="0e361-277">Názvy virtuálních počítačů, které se uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-277">The names of the virtual machines that become the cluster nodes.</span></span>
- <span data-ttu-id="0e361-278">Název pro cluster převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="0e361-278">A name for the failover cluster</span></span>
- <span data-ttu-id="0e361-279">IP adresu pro převzetí služeb při selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-279">An IP address for the failover cluster.</span></span> <span data-ttu-id="0e361-280">Můžete použít IP adresu, která nepoužívá stejnou virtuální síť Azure a podsíť jako uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-280">You can use an IP address that is not used on the same Azure virtual network and subnet as the cluster nodes.</span></span>

<span data-ttu-id="0e361-281">Následující PowerShell vytváří cluster s podporou převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-281">The following PowerShell creates a failover cluster.</span></span> <span data-ttu-id="0e361-282">Skript aktualizace s názvy uzlů (názvy virtuálních počítačů) a dostupnou IP adresu z virtuální sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="0e361-282">Update the script with the names of the nodes (the virtual machine names) and an available IP address from the Azure VNET:</span></span>

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a><span data-ttu-id="0e361-283">Vytvoření určujícího cloudu</span><span class="sxs-lookup"><span data-stu-id="0e361-283">Create a cloud witness</span></span>

<span data-ttu-id="0e361-284">Cloud s kopií clusteru je nový typ určující disk kvora clusteru, která je uložená v objektu Blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="0e361-284">Cloud Witness is a new type of cluster quorum witness stored in an Azure Storage Blob.</span></span> <span data-ttu-id="0e361-285">To eliminuje nutnost samostatné virtuálního počítače hostování určující sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="0e361-285">This removes the need of a separate VM hosting a witness share.</span></span>

1. <span data-ttu-id="0e361-286">[Vytvoření určujícího cloudu pro převzetí služeb při selhání clusteru](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="0e361-286">[Create a cloud witness for the failover cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span>

1. <span data-ttu-id="0e361-287">Vytvořte kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0e361-287">Create a blob container.</span></span>

1. <span data-ttu-id="0e361-288">Uložte přístupové klíče a adresy URL kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0e361-288">Save the access keys and the container URL.</span></span>

1. <span data-ttu-id="0e361-289">Nakonfigurujte určující disk kvora clusteru cluster převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-289">Configure the failover cluster cluster quorum witness.</span></span> <span data-ttu-id="0e361-290">V tématu, [nakonfigurovat určující disk kvora v uživatelském rozhraní]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0e361-290">See, [Configure the quorum witness in the user interface].(http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in the UI.</span></span>

### <a name="add-storage"></a><span data-ttu-id="0e361-291">Přidání úložiště</span><span class="sxs-lookup"><span data-stu-id="0e361-291">Add storage</span></span>

<span data-ttu-id="0e361-292">Disky pro S2D musí být prázdné a bez oddílů nebo jiná data.</span><span class="sxs-lookup"><span data-stu-id="0e361-292">The disks for S2D need to be empty and without partitions or other data.</span></span> <span data-ttu-id="0e361-293">Pro čištění disky podle [kroky v této příručce](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span><span class="sxs-lookup"><span data-stu-id="0e361-293">To clean disks follow [the steps in this guide](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span></span>

1. <span data-ttu-id="0e361-294">[Prostory úložiště povolení přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="0e361-294">[Enable Store Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span></span>

   <span data-ttu-id="0e361-295">Následující prostředí PowerShell umožňuje prostory úložiště – přímé.</span><span class="sxs-lookup"><span data-stu-id="0e361-295">The following PowerShell enables storage spaces direct.</span></span>  

   ```PowerShell
   Enable-ClusterS2D
   ```

   <span data-ttu-id="0e361-296">V **Správce clusteru převzetí služeb při selhání**, se zobrazí fondu úložiště.</span><span class="sxs-lookup"><span data-stu-id="0e361-296">In **Failover Cluster Manager**, you can now see the storage pool.</span></span>

1. <span data-ttu-id="0e361-297">[Vytvoření svazku](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span><span class="sxs-lookup"><span data-stu-id="0e361-297">[Create a volume](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span></span>

   <span data-ttu-id="0e361-298">Jedna z funkcí S2D je, že automaticky vytvoří fond úložiště při jejím povolením.</span><span class="sxs-lookup"><span data-stu-id="0e361-298">One of the features of S2D is that it automatically creates a storage pool when you enable it.</span></span> <span data-ttu-id="0e361-299">Nyní jste připraveni k vytvoření svazku.</span><span class="sxs-lookup"><span data-stu-id="0e361-299">You are now ready to create a volume.</span></span> <span data-ttu-id="0e361-300">Prostředí PowerShell `New-Volume` automatizuje proces vytvoření svazku, včetně formátování, přidání do clusteru a vytvoření sdíleného svazku clusteru (CSV).</span><span class="sxs-lookup"><span data-stu-id="0e361-300">The PowerShell commandlet `New-Volume` automates the volume creation process, including formatting, adding to the cluster, and creating a cluster shared volume (CSV).</span></span> <span data-ttu-id="0e361-301">Následující příklad vytvoří 800 gigabajt (GB) sdílených svazků clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-301">The following example creates an 800 gigabyte (GB) CSV.</span></span>

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   <span data-ttu-id="0e361-302">Po dokončení tohoto příkazu je připojen 800 GB svazek jako prostředku clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-302">After this command completes, an 800 GB volume is mounted as a cluster resource.</span></span> <span data-ttu-id="0e361-303">Svazek je v `C:\ClusterStorage\Volume1\`.</span><span class="sxs-lookup"><span data-stu-id="0e361-303">The volume is at `C:\ClusterStorage\Volume1\`.</span></span>

   <span data-ttu-id="0e361-304">Následující diagram znázorňuje sdílený svazek clusteru s S2D:</span><span class="sxs-lookup"><span data-stu-id="0e361-304">The following diagram shows a cluster shared volume with S2D:</span></span>

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a><span data-ttu-id="0e361-306">Krok 3: Testování převzetí služeb při selhání clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="0e361-306">Step 3: Test failover cluster failover</span></span>

<span data-ttu-id="0e361-307">Ve Správci clusteru převzetí služeb při selhání ověřte, že prostředků úložiště můžete přesunout do jiného uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-307">In Failover Cluster Manager, verify that you can move the storage resource to the other cluster node.</span></span> <span data-ttu-id="0e361-308">Pokud se můžete připojit ke clusteru převzetí služeb při selhání s **Správce clusteru převzetí služeb při selhání** a přesunout úložiště z jednoho uzlu do druhého, jste připraveni ke konfiguraci FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-308">If you can connect to the failover cluster with **Failover Cluster Manager** and move the storage from one node to the other, you are ready to configure the FCI.</span></span>

## <a name="step-4-create-sql-server-fci"></a><span data-ttu-id="0e361-309">Krok 4: Vytvoření systému SQL Server FCI</span><span class="sxs-lookup"><span data-stu-id="0e361-309">Step 4: Create SQL Server FCI</span></span>

<span data-ttu-id="0e361-310">Po nakonfigurování clusteru převzetí služeb při selhání a všechny součásti clusteru, včetně úložiště, můžete vytvořit SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-310">After you have configured the failover cluster and all cluster components including storage, you can create the SQL Server FCI.</span></span>

1. <span data-ttu-id="0e361-311">Připojte k první virtuální počítač s RDP.</span><span class="sxs-lookup"><span data-stu-id="0e361-311">Connect to the first virtual machine with RDP.</span></span>

1. <span data-ttu-id="0e361-312">V **Správce clusteru převzetí služeb při selhání**, zkontrolujte, zda jsou všechny základní prostředky clusteru na prvním virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="0e361-312">In **Failover Cluster Manager**, make sure all cluster core resources are on the first virtual machine.</span></span> <span data-ttu-id="0e361-313">V případě potřeby přesuňte všechny prostředky k tomuto virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="0e361-313">If necessary, move all resources to this virtual machine.</span></span>

1. <span data-ttu-id="0e361-314">Vyhledejte na instalačním médiu.</span><span class="sxs-lookup"><span data-stu-id="0e361-314">Locate the installation media.</span></span> <span data-ttu-id="0e361-315">Pokud virtuální počítač používá jednu z imagí Azure Marketplace, se nachází na médium nástroje `C:\SQLServer_<version number>_Full`.</span><span class="sxs-lookup"><span data-stu-id="0e361-315">If the virtual machine uses one of the Azure Marketplace images, the media is located at `C:\SQLServer_<version number>_Full`.</span></span> <span data-ttu-id="0e361-316">Klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="0e361-316">Click **Setup**.</span></span>

1. <span data-ttu-id="0e361-317">V **centrum instalace SQL serveru**, klikněte na tlačítko **instalace**.</span><span class="sxs-lookup"><span data-stu-id="0e361-317">In the **SQL Server Installation Center**, click **Installation**.</span></span>

1. <span data-ttu-id="0e361-318">Klikněte na tlačítko **nová instalace SQL serveru převzetí služeb při selhání clusteru**.</span><span class="sxs-lookup"><span data-stu-id="0e361-318">Click **New SQL Server failover cluster installation**.</span></span> <span data-ttu-id="0e361-319">Postupujte podle pokynů v průvodci a nainstalujte SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-319">Follow the instructions in the wizard to install the SQL Server FCI.</span></span>

   <span data-ttu-id="0e361-320">FCI datové adresáře musí být v úložišti clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-320">The FCI data directories need to be on clustered storage.</span></span> <span data-ttu-id="0e361-321">S S2D není sdíleného disku, ale přípojného bodu do svazku na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="0e361-321">With S2D, it's not a shared disk, but a mount point to a volume on each server.</span></span> <span data-ttu-id="0e361-322">S2D synchronizuje svazku mezi obou uzlů.</span><span class="sxs-lookup"><span data-stu-id="0e361-322">S2D synchronizes the volume between both nodes.</span></span> <span data-ttu-id="0e361-323">Svazek je zobrazen do clusteru jako sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-323">The volume is presented to the cluster as a cluster shared volume.</span></span> <span data-ttu-id="0e361-324">Použijte přípojného bodu sdíleného svazku clusteru pro data adresáře.</span><span class="sxs-lookup"><span data-stu-id="0e361-324">Use the CSV mount point for the data directories.</span></span>

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. <span data-ttu-id="0e361-326">Po dokončení průvodce se instalační program nainstaluje SQL Server FCI na prvním uzlu.</span><span class="sxs-lookup"><span data-stu-id="0e361-326">After you complete the wizard, Setup will install a SQL Server FCI on the first node.</span></span>

1. <span data-ttu-id="0e361-327">Poté, co instalační program úspěšně nainstaluje FCI na prvním uzlu, připojte ve druhém uzlu pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="0e361-327">After Setup successfully installs the FCI on the first node, connect to the second node with RDP.</span></span>

1. <span data-ttu-id="0e361-328">Otevřete **centrum instalace SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="0e361-328">Open the **SQL Server Installation Center**.</span></span> <span data-ttu-id="0e361-329">Klikněte na tlačítko **instalace**.</span><span class="sxs-lookup"><span data-stu-id="0e361-329">Click **Installation**.</span></span>

1. <span data-ttu-id="0e361-330">Klikněte na tlačítko **přidat uzel do clusteru s podporou převzetí služeb při selhání systému SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="0e361-330">Click **Add node to a SQL Server failover cluster**.</span></span> <span data-ttu-id="0e361-331">Postupujte podle pokynů v průvodci k instalaci systému SQL server a přidejte tento server FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-331">Follow the instructions in the wizard to install SQL server and add this server to the FCI.</span></span>

   >[!NOTE]
   ><span data-ttu-id="0e361-332">Pokud jste použili bitovou kopii Galerie Azure Marketplace s SQL serverem, nástroje SQL Server byly součástí bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="0e361-332">If you used an Azure Marketplace gallery image with SQL Server, SQL Server tools were included with the image.</span></span> <span data-ttu-id="0e361-333">Pokud jste nepoužili tuto bitovou kopii, nainstalujte nástroje SQL Server samostatně.</span><span class="sxs-lookup"><span data-stu-id="0e361-333">If you did not use this image, install the SQL Server tools separately.</span></span> <span data-ttu-id="0e361-334">V tématu [stáhnout SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e361-334">See [Download SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="step-5-create-azure-load-balancer"></a><span data-ttu-id="0e361-335">Krok 5: Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="0e361-335">Step 5: Create Azure load balancer</span></span>

<span data-ttu-id="0e361-336">Na virtuálních počítačích Azure použijte clustery Vyrovnávání zatížení pro uložení IP adresu, která musí být v jednom uzlu clusteru současně.</span><span class="sxs-lookup"><span data-stu-id="0e361-336">On Azure virtual machines, clusters use a load balancer to hold an IP address that needs to be on one cluster node at a time.</span></span> <span data-ttu-id="0e361-337">Nástroje pro vyrovnávání zatížení v tomto řešení obsahuje IP adresu pro SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-337">In this solution, the load balancer holds the IP address for the SQL Server FCI.</span></span>

<span data-ttu-id="0e361-338">[Vytvoření a konfigurace pro vyrovnávání zatížení Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="0e361-338">[Create and configure an Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

### <a name="create-the-load-balancer-in-the-azure-portal"></a><span data-ttu-id="0e361-339">Vytvořit nástroj pro vyrovnávání zatížení na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0e361-339">Create the load balancer in the Azure portal</span></span>

<span data-ttu-id="0e361-340">Pokud chcete vytvořit pro vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="0e361-340">To create the load balancer:</span></span>

1. <span data-ttu-id="0e361-341">Na portálu Azure přejděte do skupiny prostředků s virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="0e361-341">In the Azure portal, go to the Resource Group with the virtual machines.</span></span>

1. <span data-ttu-id="0e361-342">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="0e361-342">Click **+ Add**.</span></span> <span data-ttu-id="0e361-343">Vyhledávání na webu Marketplace pro **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="0e361-343">Search the Marketplace for **Load Balancer**.</span></span> <span data-ttu-id="0e361-344">Klikněte na tlačítko **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="0e361-344">Click **Load Balancer**.</span></span>

1. <span data-ttu-id="0e361-345">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0e361-345">Click **Create**.</span></span>

1. <span data-ttu-id="0e361-346">Konfigurace vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="0e361-346">Configure the load balancer with:</span></span>

   - <span data-ttu-id="0e361-347">**Název**: název, který identifikuje nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-347">**Name**: A name that identifies the load balancer.</span></span>
   - <span data-ttu-id="0e361-348">**Typ**: nástroje pro vyrovnávání zatížení může být veřejné nebo soukromé.</span><span class="sxs-lookup"><span data-stu-id="0e361-348">**Type**: The load balancer can be either public or private.</span></span> <span data-ttu-id="0e361-349">Nástroj pro vyrovnávání zatížení privátní je přístupná z stejnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="0e361-349">A private load balancer can be accessed from within the same VNET.</span></span> <span data-ttu-id="0e361-350">Většina Azure aplikace můžete použít nástroj pro vyrovnávání zatížení privátní.</span><span class="sxs-lookup"><span data-stu-id="0e361-350">Most Azure applications can use a private load balancer.</span></span> <span data-ttu-id="0e361-351">Pokud aplikace potřebuje přístup k systému SQL Server přímo přes Internet, použijte nástroj pro vyrovnávání zatížení veřejné.</span><span class="sxs-lookup"><span data-stu-id="0e361-351">If your application needs access to SQL Server directly over the Internet, use a public load balancer.</span></span>
   - <span data-ttu-id="0e361-352">**Virtuální síť**: stejné síti jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-352">**Virtual Network**: The same network as the virtual machines.</span></span>
   - <span data-ttu-id="0e361-353">**Podsíť**: stejné podsíti jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-353">**Subnet**: The same subnet as the virtual machines.</span></span>
   - <span data-ttu-id="0e361-354">**Privátní IP adresa**: stejnou IP adresu, který jste přiřadili k síťovému prostředku clusteru SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-354">**Private IP address**: The same IP address that you assigned to the SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="0e361-355">**předplatné**: Azure vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="0e361-355">**subscription**: Your Azure subscription.</span></span>
   - <span data-ttu-id="0e361-356">**Skupina prostředků**: použijte stejné skupině prostředků jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-356">**Resource Group**: Use the same resource group as your virtual machines.</span></span>
   - <span data-ttu-id="0e361-357">**Umístění**: použití stejného umístění Azure jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="0e361-357">**Location**: Use the same Azure location as your virtual machines.</span></span>
   <span data-ttu-id="0e361-358">Viz následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="0e361-358">See the following picture:</span></span>

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-the-load-balancer-backend-pool"></a><span data-ttu-id="0e361-360">Nakonfigurujte fond back-end pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-360">Configure the load balancer backend pool</span></span>

1. <span data-ttu-id="0e361-361">Vraťte se na skupiny prostředků Azure s virtuálními počítači a vyhledejte nové nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-361">Return to the Azure Resource Group with the virtual machines and locate the new load balancer.</span></span> <span data-ttu-id="0e361-362">Možná budete muset aktualizovat zobrazení ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e361-362">You may have to refresh the view on the Resource Group.</span></span> <span data-ttu-id="0e361-363">Klikněte na nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-363">Click the load balancer.</span></span>

1. <span data-ttu-id="0e361-364">V okně nástroje pro vyrovnávání zatížení, klikněte na **back-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="0e361-364">On the load balancer blade, click **Backend pools**.</span></span>

1. <span data-ttu-id="0e361-365">Klikněte na tlačítko **+ přidat** přidat fond back-end.</span><span class="sxs-lookup"><span data-stu-id="0e361-365">Click **+ Add** to add a backend pool.</span></span>

1. <span data-ttu-id="0e361-366">Zadejte název pro fond back-end.</span><span class="sxs-lookup"><span data-stu-id="0e361-366">Type a name for the backend pool.</span></span>

1. <span data-ttu-id="0e361-367">Klikněte na tlačítko **přidat virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="0e361-367">Click **Add a virtual machine**.</span></span>

1. <span data-ttu-id="0e361-368">Na **vyberte virtuální počítače** okně klikněte na tlačítko **zvolit skupinu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="0e361-368">On the **Choose virtual machines** blade, click **Choose an availability set**.</span></span>

1. <span data-ttu-id="0e361-369">Vyberte, že dostupnost nastavit, že jste umístili virtuální počítače v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e361-369">Choose the availability set that you placed the SQL Server virtual machines in.</span></span>

1. <span data-ttu-id="0e361-370">Na **vyberte virtuální počítače** okně klikněte na tlačítko **zvolit virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="0e361-370">On the **Choose virtual machines** blade, click **Choose the virtual machines**.</span></span>

   <span data-ttu-id="0e361-371">Portálu Azure by měl vypadat jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="0e361-371">Your Azure portal should look like the following picture:</span></span>

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. <span data-ttu-id="0e361-373">Klikněte na tlačítko **vyberte** na **vyberte virtuální počítače** okno.</span><span class="sxs-lookup"><span data-stu-id="0e361-373">Click **Select** on the **Choose virtual machines** blade.</span></span>

1. <span data-ttu-id="0e361-374">Klikněte na tlačítko **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="0e361-374">Click **OK** twice.</span></span>

### <a name="configure-a-load-balancer-health-probe"></a><span data-ttu-id="0e361-375">Konfigurace stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-375">Configure a load balancer health probe</span></span>

1. <span data-ttu-id="0e361-376">V okně nástroje pro vyrovnávání zatížení, klikněte na **testy stavu**.</span><span class="sxs-lookup"><span data-stu-id="0e361-376">On the load balancer blade, click **Health probes**.</span></span>

1. <span data-ttu-id="0e361-377">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="0e361-377">Click **+ Add**.</span></span>

1. <span data-ttu-id="0e361-378">Na **test stavu přidat** okně <a name="probe"> </a>nastavit stav testu parametry:</span><span class="sxs-lookup"><span data-stu-id="0e361-378">On the **Add health probe** blade, <a name="probe"></a>Set the health probe parameters:</span></span>

   - <span data-ttu-id="0e361-379">**Název**: název pro kontrolu stavu.</span><span class="sxs-lookup"><span data-stu-id="0e361-379">**Name**: A name for the health probe.</span></span>
   - <span data-ttu-id="0e361-380">**Protokol**: TCP.</span><span class="sxs-lookup"><span data-stu-id="0e361-380">**Protocol**: TCP.</span></span>
   - <span data-ttu-id="0e361-381">**Port**: nastavte na dostupný port TCP.</span><span class="sxs-lookup"><span data-stu-id="0e361-381">**Port**: Set to an available TCP port.</span></span> <span data-ttu-id="0e361-382">Tento port vyžaduje k portu brány firewall otevřít.</span><span class="sxs-lookup"><span data-stu-id="0e361-382">This port requires an open firewall port.</span></span> <span data-ttu-id="0e361-383">Použití [stejný port](#ports) nastavení pro test stavu v bráně firewall.</span><span class="sxs-lookup"><span data-stu-id="0e361-383">Use the [same port](#ports) you set for the health probe at the firewall.</span></span>
   - <span data-ttu-id="0e361-384">**Interval**: 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="0e361-384">**Interval**: 5 Seconds.</span></span>
   - <span data-ttu-id="0e361-385">**Prahová hodnota špatného stavu**: 2 po sobě jdoucích selhání.</span><span class="sxs-lookup"><span data-stu-id="0e361-385">**Unhealthy threshold**: 2 consecutive failures.</span></span>

1. <span data-ttu-id="0e361-386">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="0e361-386">Click OK.</span></span>

### <a name="set-load-balancing-rules"></a><span data-ttu-id="0e361-387">Nastavit pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="0e361-387">Set load balancing rules</span></span>

1. <span data-ttu-id="0e361-388">V okně nástroje pro vyrovnávání zatížení, klikněte na **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="0e361-388">On the load balancer blade, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="0e361-389">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="0e361-389">Click **+ Add**.</span></span>

1. <span data-ttu-id="0e361-390">Nastavte parametry pravidla Vyrovnávání zatížení:</span><span class="sxs-lookup"><span data-stu-id="0e361-390">Set the load balancing rules parameters:</span></span>

   - <span data-ttu-id="0e361-391">**Název**: název pravidla Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-391">**Name**: A name for the load balancing rules.</span></span>
   - <span data-ttu-id="0e361-392">**Adresa IP front-endu**: používat IP adresu pro SQL Server FCI clusteru síťovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="0e361-392">**Frontend IP address**: Use the IP address for the SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="0e361-393">**Port**: pro port TCP systému SQL Server FCI nastaveno.</span><span class="sxs-lookup"><span data-stu-id="0e361-393">**Port**: Set for the SQL Server FCI TCP port.</span></span> <span data-ttu-id="0e361-394">Výchozí instance port je 1433.</span><span class="sxs-lookup"><span data-stu-id="0e361-394">The default instance port is 1433.</span></span>
   - <span data-ttu-id="0e361-395">**Back-endový port**: Tato hodnota používá stejný port jako **Port** hodnotu, pokud povolíte **plovoucí IP adresa (přímá odpověď ze serveru)**.</span><span class="sxs-lookup"><span data-stu-id="0e361-395">**Backend port**: This value uses the same port as the **Port** value when you enable **Floating IP (direct server return)**.</span></span>
   - <span data-ttu-id="0e361-396">**Fond back-end**: použijte název fondu back-end, který jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="0e361-396">**Backend pool**: Use the backend pool name that you configured earlier.</span></span>
   - <span data-ttu-id="0e361-397">**Test stavu**: použijte Test stavu, který jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="0e361-397">**Health probe**: Use the health probe that you configured earlier.</span></span>
   - <span data-ttu-id="0e361-398">**Trvalost relace**: žádné.</span><span class="sxs-lookup"><span data-stu-id="0e361-398">**Session persistence**: None.</span></span>
   - <span data-ttu-id="0e361-399">**Časový limit (v minutách) nečinnosti**: 4.</span><span class="sxs-lookup"><span data-stu-id="0e361-399">**Idle timeout (minutes)**: 4.</span></span>
   - <span data-ttu-id="0e361-400">**Plovoucí IP adresa (přímá odpověď ze serveru)**: povoleno</span><span class="sxs-lookup"><span data-stu-id="0e361-400">**Floating IP (direct server return)**: Enabled</span></span>

1. <span data-ttu-id="0e361-401">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e361-401">Click **OK**.</span></span>

## <a name="step-6-configure-cluster-for-probe"></a><span data-ttu-id="0e361-402">Krok 6: Konfigurace clusteru pro test paměti</span><span class="sxs-lookup"><span data-stu-id="0e361-402">Step 6: Configure cluster for probe</span></span>

<span data-ttu-id="0e361-403">Nastavte parametr port testu clusteru v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0e361-403">Set the cluster probe port parameter in PowerShell.</span></span>

<span data-ttu-id="0e361-404">Pokud chcete nastavit parametr port testu clusteru, aktualizujte proměnné ve následující skript z prostředí.</span><span class="sxs-lookup"><span data-stu-id="0e361-404">To set the cluster probe port parameter, update variables in the following script from your environment.</span></span>

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "IP Address Resource Name" # the IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a><span data-ttu-id="0e361-405">Krok 7: Testování převzetí služeb při selhání FCI</span><span class="sxs-lookup"><span data-stu-id="0e361-405">Step 7: Test FCI failover</span></span>

<span data-ttu-id="0e361-406">Testovací převzetí služeb při selhání FCI k ověření fungování clusteru.</span><span class="sxs-lookup"><span data-stu-id="0e361-406">Test failover of the FCI to validate cluster functionality.</span></span> <span data-ttu-id="0e361-407">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0e361-407">Do the following steps:</span></span>

1. <span data-ttu-id="0e361-408">Připojení s jedním z uzlů clusteru SQL serveru FCI pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="0e361-408">Connect to one of the SQL Server FCI cluster nodes with RDP.</span></span>

1. <span data-ttu-id="0e361-409">Otevřete **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="0e361-409">Open **Failover Cluster Manager**.</span></span> <span data-ttu-id="0e361-410">Klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="0e361-410">Click **Roles**.</span></span> <span data-ttu-id="0e361-411">Všimněte si, který uzel vlastní roli SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-411">Notice which node owns the SQL Server FCI role.</span></span>

1. <span data-ttu-id="0e361-412">Klikněte pravým tlačítkem na roli SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-412">Right-click the SQL Server FCI role.</span></span>

1. <span data-ttu-id="0e361-413">Klikněte na tlačítko **přesunout** a klikněte na tlačítko **nejvhodnějšího uzlu**.</span><span class="sxs-lookup"><span data-stu-id="0e361-413">Click **Move** and click **Best Possible Node**.</span></span>

<span data-ttu-id="0e361-414">**Správce clusteru převzetí služeb při selhání** ukazuje přechodu do offline režimu role a její prostředky.</span><span class="sxs-lookup"><span data-stu-id="0e361-414">**Failover Cluster Manager** shows the role and its resources go offline.</span></span> <span data-ttu-id="0e361-415">Prostředky pak přesuňte a režimu online na druhém uzlu.</span><span class="sxs-lookup"><span data-stu-id="0e361-415">The resources then move and come online on the other node.</span></span>

### <a name="test-connectivity"></a><span data-ttu-id="0e361-416">Test připojení</span><span class="sxs-lookup"><span data-stu-id="0e361-416">Test connectivity</span></span>

<span data-ttu-id="0e361-417">Chcete-li otestovat připojení, přihlaste se k jinému virtuálnímu počítači ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="0e361-417">To test connectivity, log in to another virtual machine in the same virtual network.</span></span> <span data-ttu-id="0e361-418">Otevřete **SQL Server Management Studio** a připojte se k názvu SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="0e361-418">Open **SQL Server Management Studio** and connect to the SQL Server FCI name.</span></span>

>[!NOTE]
><span data-ttu-id="0e361-419">Pokud třeba, můžete [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e361-419">If necessary, you can [download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="limitations"></a><span data-ttu-id="0e361-420">Omezení</span><span class="sxs-lookup"><span data-stu-id="0e361-420">Limitations</span></span>
<span data-ttu-id="0e361-421">Na virtuálních počítačích Azure nepodporuje Microsoft koordinátoru DTC (Distributed Transaction) v instancích Fci, protože RPC port není podporována pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="0e361-421">On Azure virtual machines, Microsoft Distributed Transaction Coordinator (DTC) is not supported on FCIs because the RPC port is not supported by the load balancer.</span></span>

## <a name="see-also"></a><span data-ttu-id="0e361-422">Viz také</span><span class="sxs-lookup"><span data-stu-id="0e361-422">See Also</span></span>

[<span data-ttu-id="0e361-423">Instalační program S2D pomocí vzdálené plochy (Azure)</span><span class="sxs-lookup"><span data-stu-id="0e361-423">Setup S2D with remote desktop (Azure)</span></span>](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

<span data-ttu-id="0e361-424">[Konvergované Hyper řešení s prostory úložiště direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="0e361-424">[Hyper-converged solution with storage spaces direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span></span>

[<span data-ttu-id="0e361-425">Prostory úložiště – přímé přehled</span><span class="sxs-lookup"><span data-stu-id="0e361-425">Storage Space Direct Overview</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[<span data-ttu-id="0e361-426">Podpora systému SQL Server pro S2D</span><span class="sxs-lookup"><span data-stu-id="0e361-426">SQL Server support for S2D</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
