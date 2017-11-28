---
title: "Skupiny dostupnosti SQL serveru – virtuální počítače Azure – kurz | Microsoft Docs"
description: "Tento kurz ukazuje, jak vytvořit Server vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: 228ca9ca5fddc493d27bfd6a40df5ee7306d6aa9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a><span data-ttu-id="c36b0-103">Konfigurovat vždy na skupiny dostupnosti ve virtuálním počítači Azure ručně</span><span class="sxs-lookup"><span data-stu-id="c36b0-103">Configure Always On Availability Group in Azure VM manually</span></span>

<span data-ttu-id="c36b0-104">Tento kurz ukazuje, jak vytvořit Server vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-104">This tutorial shows how to create a SQL Server Always On Availability Group on Azure Virtual Machines.</span></span> <span data-ttu-id="c36b0-105">Dokončení kurzu vytvoří skupinu dostupnosti s repliku databáze na dva servery SQL.</span><span class="sxs-lookup"><span data-stu-id="c36b0-105">The complete tutorial creates an Availability Group with a database replica on two SQL Servers.</span></span>

<span data-ttu-id="c36b0-106">**Odhad času**: dokončení po splnění předpokladů trvá asi 30 minut.</span><span class="sxs-lookup"><span data-stu-id="c36b0-106">**Time estimate**: Takes about 30 minutes to complete once the prerequisites are met.</span></span>

<span data-ttu-id="c36b0-107">Diagram znázorňuje, co vytvoříte v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-107">The diagram illustrates what you build in the tutorial.</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a><span data-ttu-id="c36b0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c36b0-109">Prerequisites</span></span>

<span data-ttu-id="c36b0-110">Kurz předpokládá, že máte základní znalosti o SQL serveru skupin dostupnosti Always On.</span><span class="sxs-lookup"><span data-stu-id="c36b0-110">The tutorial assumes you have a basic understanding of SQL Server Always On Availability Groups.</span></span> <span data-ttu-id="c36b0-111">Pokud potřebujete další informace, přečtěte si [přehled o skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b0-111">If you need more information, see [Overview of Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span></span>

<span data-ttu-id="c36b0-112">Následující tabulka uvádí požadavky, které je potřeba provést před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="c36b0-112">The following table lists the prerequisites that you need to complete before starting this tutorial:</span></span>

|  |<span data-ttu-id="c36b0-113">Požadavek</span><span class="sxs-lookup"><span data-stu-id="c36b0-113">Requirement</span></span> |<span data-ttu-id="c36b0-114">Popis</span><span class="sxs-lookup"><span data-stu-id="c36b0-114">Description</span></span> |
|----- |----- |----- |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | <span data-ttu-id="c36b0-116">Dva servery SQL</span><span class="sxs-lookup"><span data-stu-id="c36b0-116">Two SQL Servers</span></span> | <span data-ttu-id="c36b0-117">-V nastavení dostupnosti Azure</span><span class="sxs-lookup"><span data-stu-id="c36b0-117">- In an Azure availability set</span></span> <br/> <span data-ttu-id="c36b0-118">-V jedné doméně</span><span class="sxs-lookup"><span data-stu-id="c36b0-118">- In a single domain</span></span> <br/> <span data-ttu-id="c36b0-119">-S nainstalovanou funkcí Clustering převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c36b0-119">- With Failover Clustering feature installed</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| <span data-ttu-id="c36b0-121">Windows Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-121">Windows Server</span></span> | <span data-ttu-id="c36b0-122">Sdílení souborů pro cluster s kopií clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-122">File share for cluster witness</span></span> |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c36b0-124">Účet služby SQL Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-124">SQL Server service account</span></span> | <span data-ttu-id="c36b0-125">Účet domény</span><span class="sxs-lookup"><span data-stu-id="c36b0-125">Domain account</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c36b0-127">Účet služby agenta systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-127">SQL Server Agent service account</span></span> | <span data-ttu-id="c36b0-128">Účet domény</span><span class="sxs-lookup"><span data-stu-id="c36b0-128">Domain account</span></span> |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c36b0-130">Otevřete porty brány firewall</span><span class="sxs-lookup"><span data-stu-id="c36b0-130">Firewall ports open</span></span> | <span data-ttu-id="c36b0-131">-SQL Server: **1433** pro výchozí instanci</span><span class="sxs-lookup"><span data-stu-id="c36b0-131">- SQL Server: **1433** for default instance</span></span> <br/> <span data-ttu-id="c36b0-132">-Koncový bod zrcadlení databáze: **5022** nebo jakýkoli dostupný port</span><span class="sxs-lookup"><span data-stu-id="c36b0-132">- Database mirroring endpoint: **5022** or any available port</span></span> <br/> <span data-ttu-id="c36b0-133">-Sondu nástroje pro vyrovnávání zatížení azure: **59999** nebo jakýkoli dostupný port</span><span class="sxs-lookup"><span data-stu-id="c36b0-133">- Azure load balancer probe: **59999** or any available port</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c36b0-135">Přidejte funkci Clustering převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c36b0-135">Add Failover Clustering Feature</span></span> | <span data-ttu-id="c36b0-136">Tato funkce vyžaduje oba servery SQL</span><span class="sxs-lookup"><span data-stu-id="c36b0-136">Both SQL Servers require this feature</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c36b0-138">Účet domény instalace</span><span class="sxs-lookup"><span data-stu-id="c36b0-138">Installation domain account</span></span> | <span data-ttu-id="c36b0-139">– Místní správce na každém serveru SQL</span><span class="sxs-lookup"><span data-stu-id="c36b0-139">- Local administrator on each SQL Server</span></span> <br/> <span data-ttu-id="c36b0-140">-Členem systému SQL Server pevné role serveru sysadmin pro každou instanci systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-140">- Member of SQL Server sysadmin fixed server role for each instance of SQL Server</span></span>  |


<span data-ttu-id="c36b0-141">Než začnete tento kurz, budete muset [dokončení požadované součásti pro vytváření skupin dostupnosti Always On v Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span><span class="sxs-lookup"><span data-stu-id="c36b0-141">Before you begin the tutorial, you need to [Complete prerequisites for creating Always On Availability Groups in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span></span> <span data-ttu-id="c36b0-142">Pokud tyto požadavky jsou již dokončena, můžete přejít na [vytvořením clusteru](#CreateCluster).</span><span class="sxs-lookup"><span data-stu-id="c36b0-142">If these prerequisites are completed already, you can jump to [Create Cluster](#CreateCluster).</span></span>


<span data-ttu-id="c36b0-143"><!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Vytvoření clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-143"><!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## Create the cluster</span></span>

<span data-ttu-id="c36b0-144">Po dokončení požadavky prvním krokem je vytvoření clusteru převzetí služeb při selhání Windows serveru, který obsahuje dva servery SQL Server a server s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-144">After the prerequisites are completed, the first step is to create a Windows Server Failover Cluster that includes two SQL Severs and a witness server.</span></span>  

1. <span data-ttu-id="c36b0-145">RDP k první systému SQL Server pomocí účtu domény, který je správcem na serverech SQL i na serveru s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-145">RDP to the first SQL Server using a domain account that is an administrator on both SQL Servers and the witness server.</span></span>

   >[!TIP]
   ><span data-ttu-id="c36b0-146">Pokud jste postupovali podle [požadovaný dokument](virtual-machines-windows-portal-sql-availability-group-prereq.md), vytvořili účet názvem **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-146">If you followed the [prerequisites document](virtual-machines-windows-portal-sql-availability-group-prereq.md), you created an account called **CORP\Install**.</span></span> <span data-ttu-id="c36b0-147">Pomocí tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-147">Use this account.</span></span>

2. <span data-ttu-id="c36b0-148">V **správce serveru** řídicí panel, vyberte **nástroje**a potom klikněte na **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-148">In the **Server Manager** dashboard, select **Tools**, and then click **Failover Cluster Manager**.</span></span>
3. <span data-ttu-id="c36b0-149">V levém podokně klikněte pravým tlačítkem na **Správce clusteru převzetí služeb při selhání**a potom klikněte na **vytvoření clusteru s podporou**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-149">In the left pane, right-click **Failover Cluster Manager**, and then click **Create a Cluster**.</span></span>
   <span data-ttu-id="c36b0-150">![Vytvoření clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span><span class="sxs-lookup"><span data-stu-id="c36b0-150">![Create Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span></span>
4. <span data-ttu-id="c36b0-151">V Průvodci vytvořením clusteru vytvořte jednouzlového clusteru procházení stránek s nastavením v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="c36b0-151">In the Create Cluster Wizard, create a one-node cluster by stepping through the pages with the settings in the following table:</span></span>

   | <span data-ttu-id="c36b0-152">Stránka</span><span class="sxs-lookup"><span data-stu-id="c36b0-152">Page</span></span> | <span data-ttu-id="c36b0-153">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-153">Settings</span></span> |
   | --- | --- |
   | <span data-ttu-id="c36b0-154">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c36b0-154">Before You Begin</span></span> |<span data-ttu-id="c36b0-155">Použít výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-155">Use defaults</span></span> |
   | <span data-ttu-id="c36b0-156">Vyberte servery</span><span class="sxs-lookup"><span data-stu-id="c36b0-156">Select Servers</span></span> |<span data-ttu-id="c36b0-157">Zadejte první název systému SQL Server v **zadejte název serveru** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-157">Type the first SQL Server name in **Enter server name** and click **Add**.</span></span> |
   | <span data-ttu-id="c36b0-158">Upozornění ověření</span><span class="sxs-lookup"><span data-stu-id="c36b0-158">Validation Warning</span></span> |<span data-ttu-id="c36b0-159">Vyberte **ne. I nevyžadují podporu společnosti Microsoft pro tento cluster a proto nechcete, aby ke spuštění ověřovacích testů. Po klepnutí na tlačítko Další, pokračovat ve vytváření clusteru**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-159">Select **No. I do not require support from Microsoft for this cluster, and therefore do not want to run the validation tests. When I click Next, continue Creating the cluster**.</span></span> |
   | <span data-ttu-id="c36b0-160">Přístupový bod pro správu clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-160">Access Point for Administering the Cluster</span></span> |<span data-ttu-id="c36b0-161">Zadejte název clusteru, například **SQLAGCluster1** v **název clusteru**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-161">Type a cluster name, for example **SQLAGCluster1** in **Cluster Name**.</span></span>|
   | <span data-ttu-id="c36b0-162">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="c36b0-162">Confirmation</span></span> |<span data-ttu-id="c36b0-163">Použít výchozí hodnoty, pokud nepoužíváte prostory úložiště.</span><span class="sxs-lookup"><span data-stu-id="c36b0-163">Use defaults unless you are using Storage Spaces.</span></span> <span data-ttu-id="c36b0-164">Přečtěte si poznámku za touto tabulkou.</span><span class="sxs-lookup"><span data-stu-id="c36b0-164">See the note following this table.</span></span> |

### <a name="set-the-cluster-ip-address"></a><span data-ttu-id="c36b0-165">Nastavte IP adresu clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-165">Set the cluster IP address</span></span>

1. <span data-ttu-id="c36b0-166">V **Správce clusteru převzetí služeb při selhání**, přejděte dolů k položce **základní prostředky clusteru** a rozbalte podrobnosti o clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-166">In **Failover Cluster Manager**, scroll down to **Cluster Core Resources** and expand the cluster details.</span></span> <span data-ttu-id="c36b0-167">Měli byste vidět, jak **název** a **IP adresu** prostředky v **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-167">You should see both the **Name** and the **IP Address** resources in the **Failed** state.</span></span> <span data-ttu-id="c36b0-168">Prostředek IP adresy není možné připojit online, protože clusteru je přiřazen stejnou IP adresu jako počítač sám sebe, a proto je duplicitní adresu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-168">The IP address resource cannot be brought online because the cluster is assigned the same IP address as the machine itself, therefore it is a duplicate address.</span></span>

2. <span data-ttu-id="c36b0-169">Klikněte pravým tlačítkem na chybných **IP adresu** prostředek a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-169">Right-click the failed **IP Address** resource, and then click **Properties**.</span></span>

   ![Vlastnosti clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. <span data-ttu-id="c36b0-171">Vyberte **statickou IP adresu** a zadejte dostupnou adresu z podsítě, kde je SQL Server v textovém poli Adresa.</span><span class="sxs-lookup"><span data-stu-id="c36b0-171">Select **Static IP Address** and specify an available address from subnet where the SQL Server is in the Address text box.</span></span> <span data-ttu-id="c36b0-172">Potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-172">Then, click **OK**.</span></span>
4. <span data-ttu-id="c36b0-173">V **základní prostředky clusteru** části, klikněte pravým tlačítkem na název clusteru a klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-173">In the **Cluster Core Resources** section, right-click cluster name and click **Bring Online**.</span></span> <span data-ttu-id="c36b0-174">Potom počkejte, dokud jsou obě prostředky online.</span><span class="sxs-lookup"><span data-stu-id="c36b0-174">Then, wait until both resources are online.</span></span> <span data-ttu-id="c36b0-175">Při přechodu prostředek názvu clusteru do režimu online, se nový účet počítače AD aktualizuje serveru řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="c36b0-175">When the cluster name resource comes online, it updates the DC server with a new AD computer account.</span></span> <span data-ttu-id="c36b0-176">Pomocí tohoto účtu AD později spustit služba skupiny dostupnosti v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-176">Use this AD account to run the Availability Group clustered service later.</span></span>

### <span data-ttu-id="c36b0-177"><a name="addNode"></a>Přidat SQL Server do clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-177"><a name="addNode"></a>Add the other SQL Server to cluster</span></span>

<span data-ttu-id="c36b0-178">SQL Server přidáte do clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-178">Add the other SQL Server to the cluster.</span></span>

1. <span data-ttu-id="c36b0-179">Ve stromu prohlížeče klikněte pravým tlačítkem na cluster a klikněte na tlačítko **přidat uzel**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-179">In the browser tree, right-click the cluster and click **Add Node**.</span></span>

    ![Přidání uzlu do clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. <span data-ttu-id="c36b0-181">V **Průvodce přidáním uzlu**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-181">In the **Add Node Wizard**, click **Next**.</span></span> <span data-ttu-id="c36b0-182">V **zvolit servery** stránky, přidejte druhý Server SQL.</span><span class="sxs-lookup"><span data-stu-id="c36b0-182">In the **Select Servers** page, add the second SQL Server.</span></span> <span data-ttu-id="c36b0-183">Zadejte název serveru v **zadejte název serveru** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-183">Type the server name in **Enter server name** and then click **Add**.</span></span> <span data-ttu-id="c36b0-184">Až budete hotovi, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-184">When you are done, click **Next**.</span></span>

1. <span data-ttu-id="c36b0-185">V **upozornění ověření** klikněte na tlačítko **ne** (v produkční scénář je vhodné provést testy pro ověření).</span><span class="sxs-lookup"><span data-stu-id="c36b0-185">In the **Validation Warning** page, click **No** (in a production scenario you should perform the validation tests).</span></span> <span data-ttu-id="c36b0-186">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-186">Then, click **Next**.</span></span>

8. <span data-ttu-id="c36b0-187">V **potvrzení** stránky, pokud používáte prostory úložiště, zrušte zaškrtnutí tohoto políčka s názvem bez přípony **přidat do clusteru veškeré oprávněné úložiště.**</span><span class="sxs-lookup"><span data-stu-id="c36b0-187">In the **Confirmation** page if you are using Storage Spaces, clear the checkbox labeled **Add all eligible storage to the cluster.**</span></span>

   ![Přidání uzlu potvrzení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   ><span data-ttu-id="c36b0-189">Pokud používáte prostory úložiště a není zrušte zaškrtnutí políčka **přidat do clusteru veškeré oprávněné úložiště**, systému Windows umožňuje odpojit virtuální disky během procesu clusteringu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-189">If you are using Storage Spaces and do not uncheck **Add all eligible storage to the cluster**, Windows detaches the virtual disks during the clustering process.</span></span> <span data-ttu-id="c36b0-190">V důsledku toho se v Správce disků nebo v Průzkumníku nezobrazí, dokud prostory úložiště se odebral z clusteru a znovu připojit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c36b0-190">As a result, they do not appear in Disk Manager or Explorer until the storage spaces are removed from the cluster and reattached using PowerShell.</span></span> <span data-ttu-id="c36b0-191">Prostory úložiště skupiny víc disků ve do fondů úložišť.</span><span class="sxs-lookup"><span data-stu-id="c36b0-191">Storage Spaces groups multiple disks in to storage pools.</span></span> <span data-ttu-id="c36b0-192">Další informace najdete v tématu [prostory úložiště](https://technet.microsoft.com/library/hh831739).</span><span class="sxs-lookup"><span data-stu-id="c36b0-192">For more information, see [Storage Spaces](https://technet.microsoft.com/library/hh831739).</span></span>

1. <span data-ttu-id="c36b0-193">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-193">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-194">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-194">Click **Finish**.</span></span>

   <span data-ttu-id="c36b0-195">Správce clusteru převzetí služeb při selhání ukazuje, že má nový uzel clusteru a jsou uvedené v **uzly** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-195">Failover Cluster Manager shows that your cluster has a new node and lists it in the **Nodes** container.</span></span>

10. <span data-ttu-id="c36b0-196">Odhlaste se z této relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c36b0-196">Log out of the remote desktop session.</span></span>

### <a name="add-a-cluster-quorum-file-share"></a><span data-ttu-id="c36b0-197">Přidejte sdílenou složku kvora clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-197">Add a cluster quorum file share</span></span>

<span data-ttu-id="c36b0-198">V tomto příkladu clusteru systému Windows používá sdílenou složku k vytvoření kvora clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-198">In this example the Windows cluster uses a file share to create a cluster quorum.</span></span> <span data-ttu-id="c36b0-199">Tento kurz používá většina uzlů a sdílených sdílené složky kvora.</span><span class="sxs-lookup"><span data-stu-id="c36b0-199">This tutorial uses a Node and File Share Majority quorum.</span></span> <span data-ttu-id="c36b0-200">Další informace najdete v tématu [Principy konfigurací kvora v clusteru s podporou převzetí služeb při selhání](http://technet.microsoft.com/library/cc731739.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b0-200">For more information, see [Understanding Quorum Configurations in a Failover Cluster](http://technet.microsoft.com/library/cc731739.aspx).</span></span>

1. <span data-ttu-id="c36b0-201">Připojte k serveru člen určující sdílené složky souboru s relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c36b0-201">Connect to the file share witness member server with a remote desktop session.</span></span>

1. <span data-ttu-id="c36b0-202">Na **správce serveru**, klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-202">On **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="c36b0-203">Otevřete **Správa počítače**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-203">Open **Computer Management**.</span></span>

1. <span data-ttu-id="c36b0-204">Klikněte na tlačítko **sdílených složek**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-204">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="c36b0-205">Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-205">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="c36b0-207">Použití **Průvodce vytvořením sdílené složky** vytvořit sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c36b0-207">Use **Create a Shared Folder Wizard** to create a share.</span></span>

1. <span data-ttu-id="c36b0-208">Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c36b0-208">On **Folder Path**, click **Browse** and locate or create a path for the shared folder.</span></span> <span data-ttu-id="c36b0-209">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-209">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-210">V **název, popis a nastavení** ověřte název sdílené složky a cesta.</span><span class="sxs-lookup"><span data-stu-id="c36b0-210">In **Name, Description, and Settings** verify the share name and path.</span></span> <span data-ttu-id="c36b0-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-211">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-212">Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-212">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="c36b0-213">Klikněte na tlačítko **vlastní...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-213">Click **Custom...**.</span></span>

1. <span data-ttu-id="c36b0-214">Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-214">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="c36b0-215">Ujistěte se, že účet použitý k vytvoření clusteru má plnou kontrolu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-215">Make sure that the account used to create the cluster has full control.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. <span data-ttu-id="c36b0-217">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-217">Click **OK**.</span></span>

1. <span data-ttu-id="c36b0-218">V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-218">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="c36b0-219">Klikněte na tlačítko **Dokončit** znovu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-219">Click **Finish** again.</span></span>  

1. <span data-ttu-id="c36b0-220">Odhlaste se ze serveru</span><span class="sxs-lookup"><span data-stu-id="c36b0-220">Log out of the server</span></span>

### <a name="configure-cluster-quorum"></a><span data-ttu-id="c36b0-221">Konfigurace kvora clusteru</span><span class="sxs-lookup"><span data-stu-id="c36b0-221">Configure cluster quorum</span></span>

<span data-ttu-id="c36b0-222">Dále nastavte kvorum clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-222">Next, set the cluster quorum.</span></span>

1. <span data-ttu-id="c36b0-223">Připojte k prvním uzlu clusteru pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c36b0-223">Connect to the first cluster node with remote desktop.</span></span>

1. <span data-ttu-id="c36b0-224">V **Správce clusteru převzetí služeb při selhání**, klikněte pravým tlačítkem cluster, přejděte na **další akce**a klikněte na tlačítko **konfigurovat nastavení kvora clusteru...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-224">In **Failover Cluster Manager**, right-click the cluster, point to **More Actions**, and click **Configure Cluster Quorum Settings...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. <span data-ttu-id="c36b0-226">V **Průvodce konfigurací kvora clusteru**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-226">In **Configure Cluster Quorum Wizard**, click **Next**.</span></span>

1. <span data-ttu-id="c36b0-227">V **vybrat možnosti konfigurace kvora**, zvolte **vybrat určující disk kvora**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-227">In **Select Quorum Configuration Option**, choose **Select the quorum witness**, and click **Next**.</span></span>

1. <span data-ttu-id="c36b0-228">Na **vybrat určující disk kvora**, klikněte na tlačítko **nakonfigurovat určující sdílenou složku**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-228">On **Select Quorum Witness**, click **Configure a file share witness**.</span></span>

   >[!TIP]
   ><span data-ttu-id="c36b0-229">Windows Server 2016 podporuje určujícího cloudu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-229">Windows Server 2016 supports a cloud witness.</span></span> <span data-ttu-id="c36b0-230">Pokud si zvolíte tento typ určující sdílené složky, není nutné soubor sdílet s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-230">If you choose this type of witness, you do not need a file share witness.</span></span> <span data-ttu-id="c36b0-231">Další informace najdete v tématu [nasazení cloudu určující pro Cluster s podporou převzetí služeb při selhání](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="c36b0-231">For more information, see [Deploy a cloud witness for a Failover Cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span> <span data-ttu-id="c36b0-232">Tento kurz používá určující sdílenou složku souboru, který není podporován ve starších operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="c36b0-232">This tutorial uses a file share witness, which is supported by previous operating systems.</span></span>

1. <span data-ttu-id="c36b0-233">Na **nakonfigurovat určující sdílenou složku**, zadejte cestu pro sdílenou složku, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c36b0-233">On **Configure File Share Witness**, type the path for the share you created.</span></span> <span data-ttu-id="c36b0-234">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-234">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-235">Ověřte nastavení na **potvrzení**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-235">Verify the settings on **Confirmation**.</span></span> <span data-ttu-id="c36b0-236">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-236">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-237">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-237">Click **Finish**.</span></span>

<span data-ttu-id="c36b0-238">Základní prostředky clusteru jsou nakonfigurovány s určující sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c36b0-238">The cluster core resources are configured with a file share witness.</span></span>

## <a name="enable-availability-groups"></a><span data-ttu-id="c36b0-239">Povolte skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c36b0-239">Enable Availability Groups</span></span>

<span data-ttu-id="c36b0-240">Dál povolte **skupiny dostupnosti AlwaysOn** funkce.</span><span class="sxs-lookup"><span data-stu-id="c36b0-240">Next, enable the **AlwaysOn Availability Groups** feature.</span></span> <span data-ttu-id="c36b0-241">Proveďte tyto kroky na obou serverech SQL.</span><span class="sxs-lookup"><span data-stu-id="c36b0-241">Do these steps on both SQL Servers.</span></span>

1. <span data-ttu-id="c36b0-242">Z **spustit** obrazovky, spusťte **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-242">From the **Start** screen, launch **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="c36b0-243">Ve stromu prohlížeče klikněte na tlačítko **služby SQL Server**, klikněte pravým tlačítkem **serveru SQL (MSSQLSERVER)** služby a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-243">In the browser tree, click **SQL Server Services**, then right-click the **SQL Server (MSSQLSERVER)** service and click **Properties**.</span></span>
3. <span data-ttu-id="c36b0-244">Klikněte **vysoké dostupnosti AlwaysOn** a potom vyberte **povolte skupiny dostupnosti AlwaysOn**, a to takto:</span><span class="sxs-lookup"><span data-stu-id="c36b0-244">Click the **AlwaysOn High Availability** tab, then select **Enable AlwaysOn Availability Groups**, as follows:</span></span>

    ![Povolte skupiny dostupnosti AlwaysOn](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. <span data-ttu-id="c36b0-246">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-246">Click **Apply**.</span></span> <span data-ttu-id="c36b0-247">Klikněte na tlačítko **OK** v dialogovém okně automaticky otevírané okno.</span><span class="sxs-lookup"><span data-stu-id="c36b0-247">Click **OK** in the pop-up dialog.</span></span>

5. <span data-ttu-id="c36b0-248">Restartujte službu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c36b0-248">Restart the SQL Server service.</span></span>

<span data-ttu-id="c36b0-249">Opakujte tyto kroky na SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-249">Repeat these steps on the other SQL Server.</span></span>

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for the database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for the instance of SQL Server that is used to synchronize the database replicas in the Availability Groups on that instance.

On both SQL Servers, open the firewall for the TCP port for the database mirroring endpoint.

1. On the first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In the left pane, select **Inbound Rules**. On the right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For the port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In the **Action** page, keep **Allow the connection** selected and click **Next**.
6. In the **Profile** page, accept the default settings and click **Next**.
7. In the **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in the **Name** text box, then click **Finish**.

Repeat these steps on the second SQL Server.
-------------------------->

## <a name="create-a-database-on-the-first-sql-server"></a><span data-ttu-id="c36b0-250">Vytvořit databázi na prvním serveru SQL</span><span class="sxs-lookup"><span data-stu-id="c36b0-250">Create a database on the first SQL Server</span></span>

1. <span data-ttu-id="c36b0-251">Spusťte soubor RDP k první systému SQL Server pomocí účtu domény, který je členem pevné role serveru sysadmin.</span><span class="sxs-lookup"><span data-stu-id="c36b0-251">Launch the RDP file to the first SQL Server with a domain account that is a member of sysadmin fixed server role.</span></span>
1. <span data-ttu-id="c36b0-252">Otevřete aplikaci SQL Server Management Studio a připojte se k první systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c36b0-252">Open SQL Server Management Studio and connect to the first SQL Server.</span></span>
7. <span data-ttu-id="c36b0-253">V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze** a klikněte na tlačítko **novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-253">In **Object Explorer**, right-click **Databases** and click **New Database**.</span></span>
8. <span data-ttu-id="c36b0-254">V **název databáze**, typ **MyDB1**, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-254">In **Database name**, type **MyDB1**, then click **OK**.</span></span>

### <span data-ttu-id="c36b0-255"><a name="backupshare"></a>Vytvoří sdílenou složku zálohování</span><span class="sxs-lookup"><span data-stu-id="c36b0-255"><a name="backupshare"></a> Create a backup share</span></span>

1. <span data-ttu-id="c36b0-256">Na prvním serveru SQL v **správce serveru**, klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-256">On the first SQL Server in **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="c36b0-257">Otevřete **Správa počítače**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-257">Open **Computer Management**.</span></span>

1. <span data-ttu-id="c36b0-258">Klikněte na tlačítko **sdílených složek**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-258">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="c36b0-259">Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-259">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="c36b0-261">Použití **Průvodce vytvořením sdílené složky** vytvořit sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c36b0-261">Use **Create a Shared Folder Wizard** to create a share.</span></span>

1. <span data-ttu-id="c36b0-262">Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku zálohování databáze.</span><span class="sxs-lookup"><span data-stu-id="c36b0-262">On **Folder Path**, click **Browse** and locate or create a path for the database backup shared folder.</span></span> <span data-ttu-id="c36b0-263">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-263">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-264">V **název, popis a nastavení** ověřte název sdílené složky a cesta.</span><span class="sxs-lookup"><span data-stu-id="c36b0-264">In **Name, Description, and Settings** verify the share name and path.</span></span> <span data-ttu-id="c36b0-265">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-265">Click **Next**.</span></span>

1. <span data-ttu-id="c36b0-266">Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-266">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="c36b0-267">Klikněte na tlačítko **vlastní...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-267">Click **Custom...**.</span></span>

1. <span data-ttu-id="c36b0-268">Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="c36b0-268">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="c36b0-269">Ujistěte se, že mají účty služby SQL Server a agenta systému SQL Server pro oba servery úplné řízení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-269">Make sure that the SQL Server and SQL Server Agent service accounts for both servers have full control.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. <span data-ttu-id="c36b0-271">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-271">Click **OK**.</span></span>

1. <span data-ttu-id="c36b0-272">V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-272">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="c36b0-273">Klikněte na tlačítko **Dokončit** znovu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-273">Click **Finish** again.</span></span>  

### <a name="take-a-full-backup-of-the-database"></a><span data-ttu-id="c36b0-274">Proveďte úplnou zálohu databáze</span><span class="sxs-lookup"><span data-stu-id="c36b0-274">Take a full backup of the database</span></span>

<span data-ttu-id="c36b0-275">Potřebujete zálohovat databázi nové inicializace řetězce protokolu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-275">You need to back up the new database to initialize the log chain.</span></span> <span data-ttu-id="c36b0-276">Pokud neprovedete zálohování nové databáze, nemůže být součástí skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c36b0-276">If you do not take a backup of the new database, it cannot be included in an Availability Group.</span></span>

1. <span data-ttu-id="c36b0-277">V **Průzkumník objektů**, klikněte pravým tlačítkem na databázi, přejděte na **úlohy...** , klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-277">In **Object Explorer**, right-click the database, point to **Tasks...**, click **Back Up**.</span></span>

1. <span data-ttu-id="c36b0-278">Klikněte na tlačítko **OK** provést úplné zálohování do výchozího umístění zálohy.</span><span class="sxs-lookup"><span data-stu-id="c36b0-278">Click **OK** to take a full backup to the default backup location.</span></span>

## <a name="create-the-availability-group"></a><span data-ttu-id="c36b0-279">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c36b0-279">Create the Availability Group</span></span>
<span data-ttu-id="c36b0-280">Nyní jste připraveni ke konfiguraci skupiny dostupnosti pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c36b0-280">You are now ready to configure an Availability Group using the following steps:</span></span>

* <span data-ttu-id="c36b0-281">Vytvořte databázi na prvním serveru SQL.</span><span class="sxs-lookup"><span data-stu-id="c36b0-281">Create a database on the first SQL Server.</span></span>
* <span data-ttu-id="c36b0-282">Proveďte úplné zálohování a zálohování protokolu transakcí databáze</span><span class="sxs-lookup"><span data-stu-id="c36b0-282">Take both a full backup and a transaction log backup of the database</span></span>
* <span data-ttu-id="c36b0-283">Obnovit celý a protokolu zálohování na druhý Server SQL s **NORECOVERY** možnost</span><span class="sxs-lookup"><span data-stu-id="c36b0-283">Restore the full and log backups to the second SQL Server with the **NORECOVERY** option</span></span>
* <span data-ttu-id="c36b0-284">Vytvoření skupiny dostupnosti (**AG1**) se synchronním potvrzováním, automatické převzetí služeb při selhání a čitelných místních replikách</span><span class="sxs-lookup"><span data-stu-id="c36b0-284">Create the Availability Group (**AG1**) with synchronous commit, automatic failover, and readable secondary replicas</span></span>

### <a name="create-the-availability-group"></a><span data-ttu-id="c36b0-285">Vytvoření skupiny dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="c36b0-285">Create the Availability Group:</span></span>

1. <span data-ttu-id="c36b0-286">V relaci vzdálené plochy k první systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c36b0-286">On remote desktop session to the first SQL Server.</span></span> <span data-ttu-id="c36b0-287">V **Průzkumník objektů** v aplikaci SSMS, klikněte pravým tlačítkem na **vysoké dostupnosti AlwaysOn** a klikněte na tlačítko **Průvodce novou skupinou dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-287">In **Object Explorer** in SSMS, right-click **AlwaysOn High Availability** and click **New Availability Group Wizard**.</span></span>

    ![Spusťte Průvodce novou skupinou dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. <span data-ttu-id="c36b0-289">V **ÚVOD** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-289">In the **Introduction** page, click **Next**.</span></span> <span data-ttu-id="c36b0-290">V **zadejte název skupiny dostupnosti** stránky, zadejte název skupiny dostupnosti, například **AG1**v **název skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-290">In the **Specify Availability Group Name** page, type a name for the Availability Group, for example **AG1**, in **Availability group name**.</span></span> <span data-ttu-id="c36b0-291">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-291">Click **Next**.</span></span>

    ![Průvodce novým AG, zadejte název skupiny AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. <span data-ttu-id="c36b0-293">V **vyberte databáze** , vyberte svou databázi a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-293">In the **Select Databases** page, select your database and click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="c36b0-294">Databáze splňuje předpoklady pro skupinu dostupnosti, protože jste provedli aspoň jednu úplnou zálohu na určený primární replice.</span><span class="sxs-lookup"><span data-stu-id="c36b0-294">The database meets the prerequisites for an Availability Group because you have taken at least one full backup on the intended primary replica.</span></span>

   ![Průvodce novým AG, vyberte možnost databáze](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. <span data-ttu-id="c36b0-296">V **zadejte repliky** klikněte na tlačítko **přidat repliky**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-296">In the **Specify Replicas** page, click **Add Replica**.</span></span>

   ![Průvodce novým AG, zadejte repliky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. <span data-ttu-id="c36b0-298">**Připojit k serveru** objeví dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c36b0-298">The **Connect to Server** dialog pops up.</span></span> <span data-ttu-id="c36b0-299">Zadejte název druhý server v **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-299">Type the name of the second server in **Server name**.</span></span> <span data-ttu-id="c36b0-300">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-300">Click **Connect**.</span></span>

   <span data-ttu-id="c36b0-301">Zpět v **zadejte repliky** stránky, měli byste vidět druhý server uvedený v **replik dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-301">Back in the **Specify Replicas** page, you should now see the second server listed in **Availability Replicas**.</span></span> <span data-ttu-id="c36b0-302">Repliky nakonfigurujte následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c36b0-302">Configure the replicas as follows.</span></span>

   ![Průvodce novým AG, zadejte repliky (dokončeno)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. <span data-ttu-id="c36b0-304">Klikněte na tlačítko **koncové body** zobrazíte koncový bod zrcadlení pro tuto skupinu dostupnosti databáze.</span><span class="sxs-lookup"><span data-stu-id="c36b0-304">Click **Endpoints** to see the database mirroring endpoint for this Availability Group.</span></span> <span data-ttu-id="c36b0-305">Použijte stejný port, který jste použili při nastavení [pravidlo brány firewall pro koncové body zrcadlení databáze](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="c36b0-305">Use the same port that you used when you set the [firewall rule for database mirroring endpoints](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. <span data-ttu-id="c36b0-307">V **vybrat počáteční synchronizaci dat** vyberte **úplné** a zadejte do sdíleného síťového umístění.</span><span class="sxs-lookup"><span data-stu-id="c36b0-307">In the **Select Initial Data Synchronization** page, select **Full** and specify a shared network location.</span></span> <span data-ttu-id="c36b0-308">K umístění, použijte [zálohování sdílené složky, kterou jste vytvořili](#backupshare).</span><span class="sxs-lookup"><span data-stu-id="c36b0-308">For the location, use the [backup share that you created](#backupshare).</span></span> <span data-ttu-id="c36b0-309">V příkladu byl **\\\\\<první systému SQL Server\>\Backup\**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-309">In the example it was, **\\\\\<First SQL Server\>\Backup\**.</span></span> <span data-ttu-id="c36b0-310">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-310">Click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="c36b0-311">Úplná synchronizace provede úplnou zálohu databáze na první instanci systému SQL Server a obnoví druhé instanci.</span><span class="sxs-lookup"><span data-stu-id="c36b0-311">Full synchronization takes a full backup of the database on the first instance of SQL Server and restores it to the second instance.</span></span> <span data-ttu-id="c36b0-312">Pro velké databáze úplná synchronizace nedoporučuje, protože to může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-312">For large databases, full synchronization is not recommended because it may take a long time.</span></span> <span data-ttu-id="c36b0-313">Tentokrát lze zmenšit ručně zálohování databáze a její obnovení `NO RECOVERY`.</span><span class="sxs-lookup"><span data-stu-id="c36b0-313">You can reduce this time by manually taking a backup of the database and restoring it with `NO RECOVERY`.</span></span> <span data-ttu-id="c36b0-314">Pokud byla databáze obnovena již s `NO RECOVERY` na druhém serveru SQL před konfigurací skupiny dostupnosti, vyberte **připojení pouze**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-314">If the database is already restored with `NO RECOVERY` on the second SQL Server before configuring the Availability Group, choose **Join only**.</span></span> <span data-ttu-id="c36b0-315">Pokud chcete provést zálohování po konfiguraci skupiny dostupnosti, vyberte **přeskočit počáteční data synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-315">If you want to take the backup after configuring the Availability Group, choose **Skip initial data synchronization**.</span></span>

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. <span data-ttu-id="c36b0-317">V **ověření** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-317">In the **Validation** page, click **Next**.</span></span> <span data-ttu-id="c36b0-318">Tato stránka by měl vypadat podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="c36b0-318">This page should look similar to the following image:</span></span>

    ![Průvodce novým AG, ověření](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    ><span data-ttu-id="c36b0-320">Protože jste nenakonfigurovali naslouchací proces skupiny dostupnosti je upozornění pro konfiguraci naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-320">There is a warning for the listener configuration because you have not configured an Availability Group listener.</span></span> <span data-ttu-id="c36b0-321">Toto upozornění můžete ignorovat, protože na virtuálních počítačích Azure můžete vytvořit naslouchací proces po vytvoření nástroje pro vyrovnávání zatížení Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-321">You can ignore this warning because on Azure virtual machines you create the listener after creating the Azure load balancer.</span></span>

10. <span data-ttu-id="c36b0-322">V **Souhrn** klikněte na tlačítko **Dokončit**, potom chvíli počkejte, než Průvodce nakonfiguruje do nové skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c36b0-322">In the **Summary** page, click **Finish**, then wait while the wizard configures the new Availability Group.</span></span> <span data-ttu-id="c36b0-323">V **průběh** stránky, můžete kliknout na **podrobnosti** zobrazíte podrobné průběh.</span><span class="sxs-lookup"><span data-stu-id="c36b0-323">In the **Progress** page, you can click **More details** to view the detailed progress.</span></span> <span data-ttu-id="c36b0-324">Po dokončení průvodce zkontrolujte **výsledky** a ověřte, že skupina dostupnosti úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="c36b0-324">Once the wizard is finished, inspect the **Results** page to verify that the Availability Group is successfully created.</span></span>

     ![Průvodce novým AG, výsledků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. <span data-ttu-id="c36b0-326">Klikněte na tlačítko **Zavřít** ukončíte průvodce.</span><span class="sxs-lookup"><span data-stu-id="c36b0-326">Click **Close** to exit the wizard.</span></span>

### <a name="check-the-availability-group"></a><span data-ttu-id="c36b0-327">Zkontrolujte skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c36b0-327">Check the Availability Group</span></span>

1. <span data-ttu-id="c36b0-328">V **Průzkumník objektů**, rozbalte položku **vysoké dostupnosti AlwaysOn**, pak rozbalte **skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-328">In **Object Explorer**, expand **AlwaysOn High Availability**, then expand **Availability Groups**.</span></span> <span data-ttu-id="c36b0-329">Teď byste měli vidět nové skupiny dostupnosti v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-329">You should now see the new Availability Group in this container.</span></span> <span data-ttu-id="c36b0-330">Klikněte pravým tlačítkem na skupinu dostupnosti a klikněte na tlačítko **zobrazit řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-330">Right-click the Availability Group and click **Show Dashboard**.</span></span>

   ![Řídicí panel zobrazit AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   <span data-ttu-id="c36b0-332">Vaše **řídicí panel AlwaysOn** by měl vypadat podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="c36b0-332">Your **AlwaysOn Dashboard** should look similar to this.</span></span>

   ![Řídicí panel AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   <span data-ttu-id="c36b0-334">Můžete zobrazit repliky, režim převzetí služeb při selhání každou repliku a stavu synchronizace.</span><span class="sxs-lookup"><span data-stu-id="c36b0-334">You can see the replicas, the failover mode of each replica and the synchronization state.</span></span>

2. <span data-ttu-id="c36b0-335">V **Správce clusteru převzetí služeb při selhání**, klikněte na cluster.</span><span class="sxs-lookup"><span data-stu-id="c36b0-335">In **Failover Cluster Manager**, click your cluster.</span></span> <span data-ttu-id="c36b0-336">Vyberte **role**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-336">Select **Roles**.</span></span> <span data-ttu-id="c36b0-337">Název skupiny dostupnosti, které jste použili je role v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-337">The Availability Group name you used is a role on the cluster.</span></span> <span data-ttu-id="c36b0-338">Této skupiny dostupnosti nemá IP adresu pro připojení klientů, protože jste nenakonfigurovali naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c36b0-338">That Availability Group does not have an IP address for client connections, because you did not configure a listener.</span></span> <span data-ttu-id="c36b0-339">Po vytvoření pro vyrovnávání zatížení Azure nakonfigurujete naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c36b0-339">You will configure the listener after you create an Azure load balancer.</span></span>

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > <span data-ttu-id="c36b0-341">Nepokoušejte se převzít služby skupiny dostupnosti ze Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c36b0-341">Do not try to fail over the Availability Group from the Failover Cluster Manager.</span></span> <span data-ttu-id="c36b0-342">Všechny operace převzetí služeb při selhání by měla provést uvnitř **řídicí panel AlwaysOn** v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="c36b0-342">All failover operations should be performed from within **AlwaysOn Dashboard** in SSMS.</span></span> <span data-ttu-id="c36b0-343">Další informace najdete v tématu [omezení na používání nástroje převzetí služeb při selhání clusteru Manager se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b0-343">For more information, see [Restrictions on Using The Failover Cluster Manager with Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx).</span></span>
    >

<span data-ttu-id="c36b0-344">Nyní máte skupinu dostupnosti s replikami na dvě instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c36b0-344">At this point, you have an Availability Group with replicas on two instances of SQL Server.</span></span> <span data-ttu-id="c36b0-345">Skupiny dostupnosti můžete přesouvat mezi instancemi.</span><span class="sxs-lookup"><span data-stu-id="c36b0-345">You can move the Availability Group between instances.</span></span> <span data-ttu-id="c36b0-346">Je nelze připojit ke skupině dostupnosti ještě vzhledem k tomu, že nemáte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c36b0-346">You cannot connect to the Availability Group yet because you do not have a listener.</span></span> <span data-ttu-id="c36b0-347">Naslouchací proces ve virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-347">In Azure virtual machines, the listener requires a load balancer.</span></span> <span data-ttu-id="c36b0-348">Dalším krokem je vytvoření nástroje pro vyrovnávání zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-348">The next step is to create the load balancer in Azure.</span></span>

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a><span data-ttu-id="c36b0-349">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="c36b0-349">Create an Azure load balancer</span></span>

<span data-ttu-id="c36b0-350">Skupinu dostupnosti SQL Server na virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-350">On Azure virtual machines, a SQL Server Availability Group requires a load balancer.</span></span> <span data-ttu-id="c36b0-351">Nástroje pro vyrovnávání zatížení obsahuje IP adresu pro naslouchací proces skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c36b0-351">The load balancer holds the IP address for the Availability Group listener.</span></span> <span data-ttu-id="c36b0-352">Tento oddíl shrnuje, jak vytvořit nástroj pro vyrovnávání zatížení na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-352">This section summarizes how to create the load balancer in the Azure portal.</span></span>

1. <span data-ttu-id="c36b0-353">Na portálu Azure přejděte do skupiny prostředků, kde jsou vaše servery SQL a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-353">In the Azure portal, go to the resource group where your SQL Servers are and click **+ Add**.</span></span>
2. <span data-ttu-id="c36b0-354">Vyhledejte **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-354">Search for **Load Balancer**.</span></span> <span data-ttu-id="c36b0-355">Zvolte Nástroje pro vyrovnávání zatížení, který je publikovaný microsoftem.</span><span class="sxs-lookup"><span data-stu-id="c36b0-355">Choose the load balancer published by Microsoft.</span></span>

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  <span data-ttu-id="c36b0-357">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-357">Click **Create**.</span></span>
3. <span data-ttu-id="c36b0-358">Nakonfigurujte následující parametry nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-358">Configure the following parameters for the load balancer.</span></span>

   | <span data-ttu-id="c36b0-359">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-359">Setting</span></span> | <span data-ttu-id="c36b0-360">Pole</span><span class="sxs-lookup"><span data-stu-id="c36b0-360">Field</span></span> |
   | --- | --- |
   | <span data-ttu-id="c36b0-361">**Název**</span><span class="sxs-lookup"><span data-stu-id="c36b0-361">**Name**</span></span> |<span data-ttu-id="c36b0-362">Použijte název textu pro danou službu Vyrovnávání zatížení, například **sqlLB**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-362">Use a text name for the load balancer, for example **sqlLB**.</span></span> |
   | <span data-ttu-id="c36b0-363">**Typ**</span><span class="sxs-lookup"><span data-stu-id="c36b0-363">**Type**</span></span> |<span data-ttu-id="c36b0-364">Interní</span><span class="sxs-lookup"><span data-stu-id="c36b0-364">Internal</span></span> |
   | <span data-ttu-id="c36b0-365">**Virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="c36b0-365">**Virtual network**</span></span> |<span data-ttu-id="c36b0-366">Použijte název virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-366">Use the name of the Azure virtual network.</span></span> |
   | <span data-ttu-id="c36b0-367">**Podsíť**</span><span class="sxs-lookup"><span data-stu-id="c36b0-367">**Subnet**</span></span> |<span data-ttu-id="c36b0-368">Použijte název podsítě, která je virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c36b0-368">Use the name of the subnet that the virtual machine is in.</span></span>  |
   | <span data-ttu-id="c36b0-369">**Přiřazení IP adresy**</span><span class="sxs-lookup"><span data-stu-id="c36b0-369">**IP address assignment**</span></span> |<span data-ttu-id="c36b0-370">Statická</span><span class="sxs-lookup"><span data-stu-id="c36b0-370">Static</span></span> |
   | <span data-ttu-id="c36b0-371">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="c36b0-371">**IP address**</span></span> |<span data-ttu-id="c36b0-372">Použijte dostupnou adresu z podsítě.</span><span class="sxs-lookup"><span data-stu-id="c36b0-372">Use an available address from subnet.</span></span> |
   | <span data-ttu-id="c36b0-373">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="c36b0-373">**Subscription**</span></span> |<span data-ttu-id="c36b0-374">Pomocí stejného předplatného jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c36b0-374">Use the same subscription as the virtual machine.</span></span> |
   | <span data-ttu-id="c36b0-375">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="c36b0-375">**Location**</span></span> |<span data-ttu-id="c36b0-376">Použijte stejné umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c36b0-376">Use the same location as the virtual machine.</span></span> |

   <span data-ttu-id="c36b0-377">Okno portálu Azure by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c36b0-377">The Azure portal blade should look like this:</span></span>

   ![Vytvořit nástroj pro vyrovnávání zatížení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. <span data-ttu-id="c36b0-379">Klikněte na tlačítko **vytvořit**, chcete-li vytvořit nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-379">Click **Create**, to create the load balancer.</span></span>

<span data-ttu-id="c36b0-380">Ke konfiguraci nástroje pro vyrovnávání zatížení, musíte vytvořit fond back-end, sondu a nastavte pravidla Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-380">To configure the load balancer, you need to create a backend pool, a probe, and set the load balancing rules.</span></span> <span data-ttu-id="c36b0-381">Proveďte tyto na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c36b0-381">Do these in the Azure portal.</span></span>

### <a name="add-backend-pool"></a><span data-ttu-id="c36b0-382">Přidejte fond back-end</span><span class="sxs-lookup"><span data-stu-id="c36b0-382">Add backend pool</span></span>

1. <span data-ttu-id="c36b0-383">V portálu Azure přejděte do vaší skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c36b0-383">In the Azure portal, go to your availability group.</span></span> <span data-ttu-id="c36b0-384">Možná budete muset aktualizovat zobrazení, aby najdete v části nástroje pro vyrovnávání zatížení nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="c36b0-384">You might need to refresh the view to see the newly created load balancer.</span></span>

   ![Najít nástroj pro vyrovnávání zatížení ve skupině prostředků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. <span data-ttu-id="c36b0-386">Klikněte na nástroje pro vyrovnávání zatížení, klikněte na tlačítko **back-endové fondy**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-386">Click the load balancer, click **Backend pools**, and click **+Add**.</span></span> <span data-ttu-id="c36b0-387">Nastavení fondu back-end následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c36b0-387">Set the backend pool as follows:</span></span>

   | <span data-ttu-id="c36b0-388">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-388">Setting</span></span> | <span data-ttu-id="c36b0-389">Popis</span><span class="sxs-lookup"><span data-stu-id="c36b0-389">Description</span></span> | <span data-ttu-id="c36b0-390">Příklad</span><span class="sxs-lookup"><span data-stu-id="c36b0-390">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c36b0-391">**Název**</span><span class="sxs-lookup"><span data-stu-id="c36b0-391">**Name**</span></span> | <span data-ttu-id="c36b0-392">Zadejte název text</span><span class="sxs-lookup"><span data-stu-id="c36b0-392">Type a text name</span></span> | <span data-ttu-id="c36b0-393">SQLLBBE</span><span class="sxs-lookup"><span data-stu-id="c36b0-393">SQLLBBE</span></span>
   | <span data-ttu-id="c36b0-394">**Přidružené k**</span><span class="sxs-lookup"><span data-stu-id="c36b0-394">**Associated to**</span></span> | <span data-ttu-id="c36b0-395">Vybrat ze seznamu</span><span class="sxs-lookup"><span data-stu-id="c36b0-395">Pick from list</span></span> | <span data-ttu-id="c36b0-396">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c36b0-396">Availability set</span></span>
   | <span data-ttu-id="c36b0-397">**Sady dostupnosti.**</span><span class="sxs-lookup"><span data-stu-id="c36b0-397">**Availability set**</span></span> | <span data-ttu-id="c36b0-398">Použijte název sady dostupnosti, která jsou vaše virtuální počítače serveru SQL v</span><span class="sxs-lookup"><span data-stu-id="c36b0-398">Use a name of the availability set that your SQL Server VMs are in</span></span> | <span data-ttu-id="c36b0-399">sqlAvailabilitySet</span><span class="sxs-lookup"><span data-stu-id="c36b0-399">sqlAvailabilitySet</span></span> |
   | <span data-ttu-id="c36b0-400">**Virtual Machines**</span><span class="sxs-lookup"><span data-stu-id="c36b0-400">**Virtual machines**</span></span> |<span data-ttu-id="c36b0-401">Dva názvy virtuální počítač Azure SQL Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-401">The two Azure SQL Server VM names</span></span> | <span data-ttu-id="c36b0-402">SQL Server-0, sqlserver 1</span><span class="sxs-lookup"><span data-stu-id="c36b0-402">sqlserver-0, sqlserver-1</span></span>

1. <span data-ttu-id="c36b0-403">Zadejte název pro fond back-end.</span><span class="sxs-lookup"><span data-stu-id="c36b0-403">Type the name for the back end pool.</span></span>

1. <span data-ttu-id="c36b0-404">Klikněte na tlačítko **+ přidat virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-404">Click **+ Add a virtual machine**.</span></span>

1. <span data-ttu-id="c36b0-405">Pro nastavení dostupnosti vyberte že dostupnost nastavit, že jsou servery SQL Server v.</span><span class="sxs-lookup"><span data-stu-id="c36b0-405">For the availability set, choose the availability set that the SQL Servers are in.</span></span>

1. <span data-ttu-id="c36b0-406">Pro virtuální počítače zahrnují oba servery SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c36b0-406">For virtual machines, include both of the SQL Servers.</span></span> <span data-ttu-id="c36b0-407">Nezahrnujte souborový server určující sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c36b0-407">Do not include the file share witness server.</span></span>

1. <span data-ttu-id="c36b0-408">Klikněte na tlačítko **OK** vytvoření fondu back-end.</span><span class="sxs-lookup"><span data-stu-id="c36b0-408">Click **OK** to create the backend pool.</span></span>

### <a name="set-the-probe"></a><span data-ttu-id="c36b0-409">Nastavit sonda</span><span class="sxs-lookup"><span data-stu-id="c36b0-409">Set the probe</span></span>

1. <span data-ttu-id="c36b0-410">Klikněte na nástroje pro vyrovnávání zatížení, klikněte na tlačítko **testy stavu**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-410">Click the load balancer, click **Health probes**, and click **+Add**.</span></span>

1. <span data-ttu-id="c36b0-411">Test stavu nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="c36b0-411">Set the health probe as follows:</span></span>

   | <span data-ttu-id="c36b0-412">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-412">Setting</span></span> | <span data-ttu-id="c36b0-413">Popis</span><span class="sxs-lookup"><span data-stu-id="c36b0-413">Description</span></span> | <span data-ttu-id="c36b0-414">Příklad</span><span class="sxs-lookup"><span data-stu-id="c36b0-414">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c36b0-415">**Název**</span><span class="sxs-lookup"><span data-stu-id="c36b0-415">**Name**</span></span> | <span data-ttu-id="c36b0-416">Text</span><span class="sxs-lookup"><span data-stu-id="c36b0-416">Text</span></span> | <span data-ttu-id="c36b0-417">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="c36b0-417">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="c36b0-418">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="c36b0-418">**Protocol**</span></span> | <span data-ttu-id="c36b0-419">Zvolte TCP</span><span class="sxs-lookup"><span data-stu-id="c36b0-419">Choose TCP</span></span> | <span data-ttu-id="c36b0-420">TCP</span><span class="sxs-lookup"><span data-stu-id="c36b0-420">TCP</span></span> |
   | <span data-ttu-id="c36b0-421">**Port**</span><span class="sxs-lookup"><span data-stu-id="c36b0-421">**Port**</span></span> | <span data-ttu-id="c36b0-422">Všechny nepoužívaný port.</span><span class="sxs-lookup"><span data-stu-id="c36b0-422">Any unused port</span></span> | <span data-ttu-id="c36b0-423">59999</span><span class="sxs-lookup"><span data-stu-id="c36b0-423">59999</span></span> |
   | <span data-ttu-id="c36b0-424">**Interval**</span><span class="sxs-lookup"><span data-stu-id="c36b0-424">**Interval**</span></span>  | <span data-ttu-id="c36b0-425">Množství času mezi pokusy o sondování v sekundách</span><span class="sxs-lookup"><span data-stu-id="c36b0-425">The amount of time between probe attempts in seconds</span></span> |<span data-ttu-id="c36b0-426">5</span><span class="sxs-lookup"><span data-stu-id="c36b0-426">5</span></span> |
   | <span data-ttu-id="c36b0-427">**Prahová hodnota špatného stavu**</span><span class="sxs-lookup"><span data-stu-id="c36b0-427">**Unhealthy threshold**</span></span> | <span data-ttu-id="c36b0-428">Počet po sobě jdoucích kontroly chyb, které musí být stejné pro virtuální počítač, aby byla považována za není v pořádku</span><span class="sxs-lookup"><span data-stu-id="c36b0-428">The number of consecutive probe failures that must occur for a virtual machine to be considered unhealthy</span></span>  | <span data-ttu-id="c36b0-429">2</span><span class="sxs-lookup"><span data-stu-id="c36b0-429">2</span></span> |

1. <span data-ttu-id="c36b0-430">Klikněte na tlačítko **OK** nastavit test stavu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-430">Click **OK** to set the health probe.</span></span>

### <a name="set-the-load-balancing-rules"></a><span data-ttu-id="c36b0-431">Nastavit pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="c36b0-431">Set the load balancing rules</span></span>

1. <span data-ttu-id="c36b0-432">Klikněte na nástroje pro vyrovnávání zatížení, klikněte na tlačítko **pravidla Vyrovnávání zatížení**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-432">Click the load balancer, click **Load balancing rules**, and click **+Add**.</span></span>

1. <span data-ttu-id="c36b0-433">Nastavte pravidla takto Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-433">Set the load balancing rules as follows.</span></span>
   | <span data-ttu-id="c36b0-434">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c36b0-434">Setting</span></span> | <span data-ttu-id="c36b0-435">Popis</span><span class="sxs-lookup"><span data-stu-id="c36b0-435">Description</span></span> | <span data-ttu-id="c36b0-436">Příklad</span><span class="sxs-lookup"><span data-stu-id="c36b0-436">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c36b0-437">**Název**</span><span class="sxs-lookup"><span data-stu-id="c36b0-437">**Name**</span></span> | <span data-ttu-id="c36b0-438">Text</span><span class="sxs-lookup"><span data-stu-id="c36b0-438">Text</span></span> | <span data-ttu-id="c36b0-439">SQLAlwaysOnEndPointListener</span><span class="sxs-lookup"><span data-stu-id="c36b0-439">SQLAlwaysOnEndPointListener</span></span> |
   | <span data-ttu-id="c36b0-440">**Adresa IP front-endu**</span><span class="sxs-lookup"><span data-stu-id="c36b0-440">**Frontend IP address**</span></span> | <span data-ttu-id="c36b0-441">Zvolte adresu</span><span class="sxs-lookup"><span data-stu-id="c36b0-441">Choose an address</span></span> |<span data-ttu-id="c36b0-442">Použijte adresu, kterou jste vytvořili, když jste vytvořili pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-442">Use the address that you created when you created the load balancer.</span></span> |
   | <span data-ttu-id="c36b0-443">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="c36b0-443">**Protocol**</span></span> | <span data-ttu-id="c36b0-444">Zvolte TCP</span><span class="sxs-lookup"><span data-stu-id="c36b0-444">Choose TCP</span></span> |<span data-ttu-id="c36b0-445">TCP</span><span class="sxs-lookup"><span data-stu-id="c36b0-445">TCP</span></span> |
   | <span data-ttu-id="c36b0-446">**Port**</span><span class="sxs-lookup"><span data-stu-id="c36b0-446">**Port**</span></span> | <span data-ttu-id="c36b0-447">Použít port pro instanci systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c36b0-447">Use the port for the SQL Server instance</span></span> | <span data-ttu-id="c36b0-448">1433</span><span class="sxs-lookup"><span data-stu-id="c36b0-448">1433</span></span> |
   | <span data-ttu-id="c36b0-449">**Back-endový Port**</span><span class="sxs-lookup"><span data-stu-id="c36b0-449">**Backend Port**</span></span> | <span data-ttu-id="c36b0-450">Toto pole se nepoužívá při plovoucí IP adresa je nastavena pro přímý server návratový</span><span class="sxs-lookup"><span data-stu-id="c36b0-450">This field is not used when Floating IP is set for direct server return</span></span> | <span data-ttu-id="c36b0-451">1433</span><span class="sxs-lookup"><span data-stu-id="c36b0-451">1433</span></span> |
   | <span data-ttu-id="c36b0-452">**Test**</span><span class="sxs-lookup"><span data-stu-id="c36b0-452">**Probe**</span></span> |<span data-ttu-id="c36b0-453">Název, který jste zadali pro kontrolu</span><span class="sxs-lookup"><span data-stu-id="c36b0-453">The name you specified for the probe</span></span> | <span data-ttu-id="c36b0-454">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="c36b0-454">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="c36b0-455">**Trvalost relace**</span><span class="sxs-lookup"><span data-stu-id="c36b0-455">**Session Persistence**</span></span> | <span data-ttu-id="c36b0-456">Rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="c36b0-456">Drop down list</span></span> | <span data-ttu-id="c36b0-457">**None**</span><span class="sxs-lookup"><span data-stu-id="c36b0-457">**None**</span></span> |
   | <span data-ttu-id="c36b0-458">**Časový limit nečinnosti**</span><span class="sxs-lookup"><span data-stu-id="c36b0-458">**Idle Timeout**</span></span> | <span data-ttu-id="c36b0-459">Otevřete minut pro uchování připojení TCP</span><span class="sxs-lookup"><span data-stu-id="c36b0-459">Minutes to keep a TCP connection open</span></span> | <span data-ttu-id="c36b0-460">4</span><span class="sxs-lookup"><span data-stu-id="c36b0-460">4</span></span> |
   | <span data-ttu-id="c36b0-461">**Plovoucí IP adresa (přímá odpověď ze serveru)**</span><span class="sxs-lookup"><span data-stu-id="c36b0-461">**Floating IP (direct server return)**</span></span> | |<span data-ttu-id="c36b0-462">Povoleno</span><span class="sxs-lookup"><span data-stu-id="c36b0-462">Enabled</span></span> |

   > [!WARNING]
   > <span data-ttu-id="c36b0-463">Přímá odpověď ze serveru se nastavuje během vytváření.</span><span class="sxs-lookup"><span data-stu-id="c36b0-463">Direct server return is set during creation.</span></span> <span data-ttu-id="c36b0-464">Nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="c36b0-464">It cannot be changed.</span></span>

1. <span data-ttu-id="c36b0-465">Klikněte na tlačítko **OK** nastavit pravidla Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-465">Click **OK** to set the load balancing rules.</span></span>

## <span data-ttu-id="c36b0-466"><a name="configure-listener"></a>Konfigurace naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="c36b0-466"><a name="configure-listener"></a> Configure the listener</span></span>

<span data-ttu-id="c36b0-467">Dalším krokem je konfigurace naslouchací proces skupiny dostupnosti v clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c36b0-467">The next thing to do is to configure an Availability Group listener on the failover cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c36b0-468">Tento kurz ukazuje, jak vytvořit jeden naslouchací proces - s jednu ILB IP adresu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-468">This tutorial shows how to create a single listener - with one ILB IP address.</span></span> <span data-ttu-id="c36b0-469">Pokud chcete vytvořit jeden nebo více naslouchací procesy pomocí jednoho nebo více IP adres, přečtěte si téma [skupiny dostupnosti vytvořit naslouchací proces a nástroj pro vyrovnávání zatížení | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c36b0-469">To create one or more listeners using one or more IP addresses, see [Create Availability Group listener and load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a><span data-ttu-id="c36b0-470">Port naslouchacího procesu sady</span><span class="sxs-lookup"><span data-stu-id="c36b0-470">Set listener port</span></span>

<span data-ttu-id="c36b0-471">V systému SQL Server Management Studio nastavte port naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="c36b0-471">In SQL Server Management Studio, set the listener port.</span></span>

1. <span data-ttu-id="c36b0-472">Spusťte SQL Server Management Studio a připojte se k primární replice.</span><span class="sxs-lookup"><span data-stu-id="c36b0-472">Launch SQL Server Management Studio and connect to the primary replica.</span></span>

1. <span data-ttu-id="c36b0-473">Přejděte na **AlwaysOn vysokou dostupnost** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-473">Navigate to **AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span>

1. <span data-ttu-id="c36b0-474">Teď byste měli vidět název naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c36b0-474">You should now see the listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="c36b0-475">Klikněte pravým tlačítkem na název naslouchacího procesu a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-475">Right-click the listener name and click **Properties**.</span></span>

1. <span data-ttu-id="c36b0-476">V **Port** pole, zadejte číslo portu pro naslouchací proces skupiny dostupnosti pomocí $EndpointPort jste použili předtím (1433 se výchozí nastavení), pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c36b0-476">In the **Port** box, specify the port number for the Availability Group listener by using the $EndpointPort you used earlier (1433 was the default), then click **OK**.</span></span>

<span data-ttu-id="c36b0-477">Nyní máte skupinu dostupnosti SQL Server na virtuálních počítačích Azure spuštěna v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c36b0-477">You now have a SQL Server Availability Group in Azure virtual machines running in Resource Manager mode.</span></span>

## <a name="test-connection-to-listener"></a><span data-ttu-id="c36b0-478">Testovací připojení pro naslouchací proces</span><span class="sxs-lookup"><span data-stu-id="c36b0-478">Test connection to listener</span></span>

<span data-ttu-id="c36b0-479">K testování připojení:</span><span class="sxs-lookup"><span data-stu-id="c36b0-479">To test the connection:</span></span>

1. <span data-ttu-id="c36b0-480">RDP k systému SQL Server, který je ve stejné virtuální síti, ale není vlastníkem repliky.</span><span class="sxs-lookup"><span data-stu-id="c36b0-480">RDP to a SQL Server that is in the same virtual network, but does not own the replica.</span></span> <span data-ttu-id="c36b0-481">Můžete použít SQL Server v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c36b0-481">You can use the other SQL Server in the cluster.</span></span>

1. <span data-ttu-id="c36b0-482">Použití **sqlcmd** nástroj k testování připojení.</span><span class="sxs-lookup"><span data-stu-id="c36b0-482">Use **sqlcmd** utility to test the connection.</span></span> <span data-ttu-id="c36b0-483">Například následující skript vytvoří **sqlcmd** připojení k primární replice prostřednictvím naslouchací proces s ověřováním systému Windows:</span><span class="sxs-lookup"><span data-stu-id="c36b0-483">For example, the following script establishes a **sqlcmd** connection to the primary replica through the listener with Windows authentication:</span></span>

    ```
    sqlcmd -S <listenerName> -E
    ```

    <span data-ttu-id="c36b0-484">Pokud naslouchací proces používá jiný port než výchozí port (1433), zadejte port v připojovacím řetězci.</span><span class="sxs-lookup"><span data-stu-id="c36b0-484">If the listener is using a port other than the default port (1433), specify the port in the connection string.</span></span> <span data-ttu-id="c36b0-485">Například následující příkaz sqlcmd připojí k naslouchání na portu 1435:</span><span class="sxs-lookup"><span data-stu-id="c36b0-485">For example, the following sqlcmd command connects to a listener at port 1435:</span></span>

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="c36b0-486">Připojení SQLCMD se automaticky připojí na kteroukoli instanci systému SQL Server hostitelem primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c36b0-486">The SQLCMD connection automatically connects to whichever instance of SQL Server hosts the primary replica.</span></span>

> [!TIP]
> <span data-ttu-id="c36b0-487">Ujistěte se, že je port, který zadáte otevřen v bráně firewall oba servery SQL.</span><span class="sxs-lookup"><span data-stu-id="c36b0-487">Make sure that the port you specify is open on the firewall of both SQL Servers.</span></span> <span data-ttu-id="c36b0-488">Oba servery vyžadují příchozí pravidlo pro port TCP, který používáte.</span><span class="sxs-lookup"><span data-stu-id="c36b0-488">Both servers require an inbound rule for the TCP port that you use.</span></span> <span data-ttu-id="c36b0-489">Další informace najdete v tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx).</span><span class="sxs-lookup"><span data-stu-id="c36b0-489">For more information, see [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx).</span></span>
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by the way” info, an Important is info users need to complete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is the second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note the format for documenting the UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult to maintain. Highlight areas you are referring to in red.*-->

<!--**No. of steps**: *Make sure the number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a><span data-ttu-id="c36b0-490">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c36b0-490">Next steps</span></span>

- <span data-ttu-id="c36b0-491">[Přidat IP adresu nástroji pro vyrovnávání zatížení pro skupinu dostupnosti druhý](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span><span class="sxs-lookup"><span data-stu-id="c36b0-491">[Add an IP address to a load balancer for a second availability group](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span></span>
