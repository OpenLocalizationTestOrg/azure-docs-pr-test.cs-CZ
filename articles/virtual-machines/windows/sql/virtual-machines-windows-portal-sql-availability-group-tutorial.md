---
title: "aaaSQL skupin dostupnosti serveru – virtuální počítače Azure – kurz | Microsoft Docs"
description: "Tento kurz ukazuje, jak toocreate serveru vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure."
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
ms.openlocfilehash: 65b4210b0f851828a32a02053b03e4b8d469ba4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a><span data-ttu-id="c88ce-103">Konfigurovat vždy na skupiny dostupnosti ve virtuálním počítači Azure ručně</span><span class="sxs-lookup"><span data-stu-id="c88ce-103">Configure Always On Availability Group in Azure VM manually</span></span>

<span data-ttu-id="c88ce-104">Tento kurz ukazuje, jak toocreate serveru vždy na skupinu dostupnosti SQL ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="c88ce-104">This tutorial shows how toocreate a SQL Server Always On Availability Group on Azure Virtual Machines.</span></span> <span data-ttu-id="c88ce-105">dokončení kurzu Hello vytvoří skupinu dostupnosti s repliku databáze na dva servery SQL.</span><span class="sxs-lookup"><span data-stu-id="c88ce-105">hello complete tutorial creates an Availability Group with a database replica on two SQL Servers.</span></span>

<span data-ttu-id="c88ce-106">**Odhad času**: trvá asi 30 minut toocomplete, jakmile jsou splněny požadavky hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-106">**Time estimate**: Takes about 30 minutes toocomplete once hello prerequisites are met.</span></span>

<span data-ttu-id="c88ce-107">Hello diagram znázorňuje, co vytvoříte v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-107">hello diagram illustrates what you build in hello tutorial.</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a><span data-ttu-id="c88ce-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c88ce-109">Prerequisites</span></span>

<span data-ttu-id="c88ce-110">Hello kurz předpokládá, že máte základní znalosti o SQL serveru skupin dostupnosti Always On.</span><span class="sxs-lookup"><span data-stu-id="c88ce-110">hello tutorial assumes you have a basic understanding of SQL Server Always On Availability Groups.</span></span> <span data-ttu-id="c88ce-111">Pokud potřebujete další informace, přečtěte si [přehled o skupin dostupnosti Always On (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span><span class="sxs-lookup"><span data-stu-id="c88ce-111">If you need more information, see [Overview of Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).</span></span>

<span data-ttu-id="c88ce-112">Hello následující tabulka uvádí hello požadavky, je nutné toocomplete před zahájením tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="c88ce-112">hello following table lists hello prerequisites that you need toocomplete before starting this tutorial:</span></span>

|  |<span data-ttu-id="c88ce-113">Požadavek</span><span class="sxs-lookup"><span data-stu-id="c88ce-113">Requirement</span></span> |<span data-ttu-id="c88ce-114">Popis</span><span class="sxs-lookup"><span data-stu-id="c88ce-114">Description</span></span> |
|----- |----- |----- |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | <span data-ttu-id="c88ce-116">Dva servery SQL</span><span class="sxs-lookup"><span data-stu-id="c88ce-116">Two SQL Servers</span></span> | <span data-ttu-id="c88ce-117">-V nastavení dostupnosti Azure</span><span class="sxs-lookup"><span data-stu-id="c88ce-117">- In an Azure availability set</span></span> <br/> <span data-ttu-id="c88ce-118">-V jedné doméně</span><span class="sxs-lookup"><span data-stu-id="c88ce-118">- In a single domain</span></span> <br/> <span data-ttu-id="c88ce-119">-S nainstalovanou funkcí Clustering převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c88ce-119">- With Failover Clustering feature installed</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| <span data-ttu-id="c88ce-121">Windows Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-121">Windows Server</span></span> | <span data-ttu-id="c88ce-122">Sdílení souborů pro cluster s kopií clusteru</span><span class="sxs-lookup"><span data-stu-id="c88ce-122">File share for cluster witness</span></span> |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c88ce-124">Účet služby SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-124">SQL Server service account</span></span> | <span data-ttu-id="c88ce-125">Účet domény</span><span class="sxs-lookup"><span data-stu-id="c88ce-125">Domain account</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c88ce-127">Účet služby agenta systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-127">SQL Server Agent service account</span></span> | <span data-ttu-id="c88ce-128">Účet domény</span><span class="sxs-lookup"><span data-stu-id="c88ce-128">Domain account</span></span> |  
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c88ce-130">Otevřete porty brány firewall</span><span class="sxs-lookup"><span data-stu-id="c88ce-130">Firewall ports open</span></span> | <span data-ttu-id="c88ce-131">-SQL Server: **1433** pro výchozí instanci</span><span class="sxs-lookup"><span data-stu-id="c88ce-131">- SQL Server: **1433** for default instance</span></span> <br/> <span data-ttu-id="c88ce-132">-Koncový bod zrcadlení databáze: **5022** nebo jakýkoli dostupný port</span><span class="sxs-lookup"><span data-stu-id="c88ce-132">- Database mirroring endpoint: **5022** or any available port</span></span> <br/> <span data-ttu-id="c88ce-133">-Sondu nástroje pro vyrovnávání zatížení azure: **59999** nebo jakýkoli dostupný port</span><span class="sxs-lookup"><span data-stu-id="c88ce-133">- Azure load balancer probe: **59999** or any available port</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c88ce-135">Přidejte funkci Clustering převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c88ce-135">Add Failover Clustering Feature</span></span> | <span data-ttu-id="c88ce-136">Tato funkce vyžaduje oba servery SQL</span><span class="sxs-lookup"><span data-stu-id="c88ce-136">Both SQL Servers require this feature</span></span> |
|![hranaté](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|<span data-ttu-id="c88ce-138">Účet domény instalace</span><span class="sxs-lookup"><span data-stu-id="c88ce-138">Installation domain account</span></span> | <span data-ttu-id="c88ce-139">– Místní správce na každém serveru SQL</span><span class="sxs-lookup"><span data-stu-id="c88ce-139">- Local administrator on each SQL Server</span></span> <br/> <span data-ttu-id="c88ce-140">-Členem systému SQL Server pevné role serveru sysadmin pro každou instanci systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-140">- Member of SQL Server sysadmin fixed server role for each instance of SQL Server</span></span>  |


<span data-ttu-id="c88ce-141">Než začnete hello kurz, musíte příliš[dokončení požadované součásti pro vytváření skupin dostupnosti Always On v Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span><span class="sxs-lookup"><span data-stu-id="c88ce-141">Before you begin hello tutorial, you need too[Complete prerequisites for creating Always On Availability Groups in Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md).</span></span> <span data-ttu-id="c88ce-142">Pokud tyto požadavky jsou již byla dokončena, můžete přeskočit příliš[vytvořením clusteru](#CreateCluster).</span><span class="sxs-lookup"><span data-stu-id="c88ce-142">If these prerequisites are completed already, you can jump too[Create Cluster](#CreateCluster).</span></span>


<span data-ttu-id="c88ce-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
##Vytvoření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-143"><!--**Procedure**: *This is hello first “step”. Make titles H2’s and short and clear – H2’s appear in hello right pane on hello web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## Create hello cluster</span></span>

<span data-ttu-id="c88ce-144">Po hello, které požadavky jsou dokončeny je prvním krokem hello toocreate clusteru převzetí služeb při selhání Windows serveru, který obsahuje dva servery SQL Server a server s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-144">After hello prerequisites are completed, hello first step is toocreate a Windows Server Failover Cluster that includes two SQL Severs and a witness server.</span></span>  

1. <span data-ttu-id="c88ce-145">RDP toohello první systému SQL Server pomocí účtu domény, který je správcem na servery SQL a hello monitorovací server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-145">RDP toohello first SQL Server using a domain account that is an administrator on both SQL Servers and hello witness server.</span></span>

   >[!TIP]
   ><span data-ttu-id="c88ce-146">Pokud jste postupovali podle hello [požadovaný dokument](virtual-machines-windows-portal-sql-availability-group-prereq.md), vytvořili účet názvem **CORP\Install**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-146">If you followed hello [prerequisites document](virtual-machines-windows-portal-sql-availability-group-prereq.md), you created an account called **CORP\Install**.</span></span> <span data-ttu-id="c88ce-147">Pomocí tohoto účtu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-147">Use this account.</span></span>

2. <span data-ttu-id="c88ce-148">V hello **správce serveru** řídicí panel, vyberte **nástroje**a potom klikněte na **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-148">In hello **Server Manager** dashboard, select **Tools**, and then click **Failover Cluster Manager**.</span></span>
3. <span data-ttu-id="c88ce-149">V levém podokně hello, klikněte pravým tlačítkem na **Správce clusteru převzetí služeb při selhání**a potom klikněte na **vytvoření clusteru s podporou**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-149">In hello left pane, right-click **Failover Cluster Manager**, and then click **Create a Cluster**.</span></span>
   <span data-ttu-id="c88ce-150">![Vytvoření clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span><span class="sxs-lookup"><span data-stu-id="c88ce-150">![Create Cluster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)</span></span>
4. <span data-ttu-id="c88ce-151">V hello Průvodce vytvořením clusteru, vytvořit cluster s jedním uzlem a procházení hello stránky s nastaveními hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c88ce-151">In hello Create Cluster Wizard, create a one-node cluster by stepping through hello pages with hello settings in hello following table:</span></span>

   | <span data-ttu-id="c88ce-152">Stránka</span><span class="sxs-lookup"><span data-stu-id="c88ce-152">Page</span></span> | <span data-ttu-id="c88ce-153">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-153">Settings</span></span> |
   | --- | --- |
   | <span data-ttu-id="c88ce-154">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c88ce-154">Before You Begin</span></span> |<span data-ttu-id="c88ce-155">Použít výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-155">Use defaults</span></span> |
   | <span data-ttu-id="c88ce-156">Vyberte servery</span><span class="sxs-lookup"><span data-stu-id="c88ce-156">Select Servers</span></span> |<span data-ttu-id="c88ce-157">Typ hello název první SQL serveru v **zadejte název serveru** a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-157">Type hello first SQL Server name in **Enter server name** and click **Add**.</span></span> |
   | <span data-ttu-id="c88ce-158">Upozornění ověření</span><span class="sxs-lookup"><span data-stu-id="c88ce-158">Validation Warning</span></span> |<span data-ttu-id="c88ce-159">Vyberte **ne. I nevyžadují podporu společnosti Microsoft pro tento cluster a proto nechcete, aby toorun hello ověřovací testy. Po klepnutí na tlačítko Další, pokračovat ve vytváření clusteru hello**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-159">Select **No. I do not require support from Microsoft for this cluster, and therefore do not want toorun hello validation tests. When I click Next, continue Creating hello cluster**.</span></span> |
   | <span data-ttu-id="c88ce-160">Přístupový bod pro hello Správa clusteru</span><span class="sxs-lookup"><span data-stu-id="c88ce-160">Access Point for Administering hello Cluster</span></span> |<span data-ttu-id="c88ce-161">Zadejte název clusteru, například **SQLAGCluster1** v **název clusteru**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-161">Type a cluster name, for example **SQLAGCluster1** in **Cluster Name**.</span></span>|
   | <span data-ttu-id="c88ce-162">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="c88ce-162">Confirmation</span></span> |<span data-ttu-id="c88ce-163">Použít výchozí hodnoty, pokud nepoužíváte prostory úložiště.</span><span class="sxs-lookup"><span data-stu-id="c88ce-163">Use defaults unless you are using Storage Spaces.</span></span> <span data-ttu-id="c88ce-164">Další informace v poznámce hello za touto tabulkou.</span><span class="sxs-lookup"><span data-stu-id="c88ce-164">See hello note following this table.</span></span> |

### <a name="set-hello-cluster-ip-address"></a><span data-ttu-id="c88ce-165">Nastavte IP adresu clusteru hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-165">Set hello cluster IP address</span></span>

1. <span data-ttu-id="c88ce-166">V **Správce clusteru převzetí služeb při selhání**, posuňte se dolů příliš**základní prostředky clusteru** a rozbalte podrobnosti o clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-166">In **Failover Cluster Manager**, scroll down too**Cluster Core Resources** and expand hello cluster details.</span></span> <span data-ttu-id="c88ce-167">Měli byste vidět obou hello **název** a hello **IP adresu** prostředky v hello **se nezdařilo** stavu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-167">You should see both hello **Name** and hello **IP Address** resources in hello **Failed** state.</span></span> <span data-ttu-id="c88ce-168">Hello prostředek IP adresy nelze do online režimu protože hello clusteru je přiřazen hello stejné IP adres jako hello počítač sám sebe, a proto je duplicitní adresu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-168">hello IP address resource cannot be brought online because hello cluster is assigned hello same IP address as hello machine itself, therefore it is a duplicate address.</span></span>

2. <span data-ttu-id="c88ce-169">Klikněte pravým tlačítkem na hello se nezdařilo **IP adresu** prostředek a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-169">Right-click hello failed **IP Address** resource, and then click **Properties**.</span></span>

   ![Vlastnosti clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. <span data-ttu-id="c88ce-171">Vyberte **statickou IP adresu** a zadejte dostupnou adresu z podsítě, je-li hello systému SQL Server hello adresu textové pole.</span><span class="sxs-lookup"><span data-stu-id="c88ce-171">Select **Static IP Address** and specify an available address from subnet where hello SQL Server is in hello Address text box.</span></span> <span data-ttu-id="c88ce-172">Potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-172">Then, click **OK**.</span></span>
4. <span data-ttu-id="c88ce-173">V hello **základní prostředky clusteru** části, klikněte pravým tlačítkem na název clusteru a klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-173">In hello **Cluster Core Resources** section, right-click cluster name and click **Bring Online**.</span></span> <span data-ttu-id="c88ce-174">Potom počkejte, dokud jsou obě prostředky online.</span><span class="sxs-lookup"><span data-stu-id="c88ce-174">Then, wait until both resources are online.</span></span> <span data-ttu-id="c88ce-175">Pokud prostředek názvu clusteru hello přechodu do režimu online, se nový účet počítače AD aktualizuje hello serveru řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="c88ce-175">When hello cluster name resource comes online, it updates hello DC server with a new AD computer account.</span></span> <span data-ttu-id="c88ce-176">Pomocí této AD účet toorun hello později služby skupiny dostupnosti v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-176">Use this AD account toorun hello Availability Group clustered service later.</span></span>

### <span data-ttu-id="c88ce-177"><a name="addNode"></a>Přidat další toocluster systému SQL Server hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-177"><a name="addNode"></a>Add hello other SQL Server toocluster</span></span>

<span data-ttu-id="c88ce-178">Přidat hello jiných toohello cluster serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-178">Add hello other SQL Server toohello cluster.</span></span>

1. <span data-ttu-id="c88ce-179">Ve stromu hello prohlížeče, klikněte pravým tlačítkem hello cluster a klikněte na tlačítko **přidat uzel**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-179">In hello browser tree, right-click hello cluster and click **Add Node**.</span></span>

    ![Přidat toohello uzlu clusteru](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. <span data-ttu-id="c88ce-181">V hello **Průvodce přidáním uzlu**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-181">In hello **Add Node Wizard**, click **Next**.</span></span> <span data-ttu-id="c88ce-182">V hello **zvolit servery** přidejte hello druhý Server SQL.</span><span class="sxs-lookup"><span data-stu-id="c88ce-182">In hello **Select Servers** page, add hello second SQL Server.</span></span> <span data-ttu-id="c88ce-183">Zadejte název serveru hello v **zadejte název serveru** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-183">Type hello server name in **Enter server name** and then click **Add**.</span></span> <span data-ttu-id="c88ce-184">Až budete hotovi, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-184">When you are done, click **Next**.</span></span>

1. <span data-ttu-id="c88ce-185">V hello **upozornění ověření** klikněte na tlačítko **ne** (v případě produkční měli byste provést testy pro ověření hello).</span><span class="sxs-lookup"><span data-stu-id="c88ce-185">In hello **Validation Warning** page, click **No** (in a production scenario you should perform hello validation tests).</span></span> <span data-ttu-id="c88ce-186">Pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-186">Then, click **Next**.</span></span>

8. <span data-ttu-id="c88ce-187">V hello **potvrzení** stránky, pokud používáte prostory úložiště, zrušte hello zaškrtávací políčko s názvem bez přípony **přidejte všechna vhodná úložiště toohello cluster.**</span><span class="sxs-lookup"><span data-stu-id="c88ce-187">In hello **Confirmation** page if you are using Storage Spaces, clear hello checkbox labeled **Add all eligible storage toohello cluster.**</span></span>

   ![Přidání uzlu potvrzení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   ><span data-ttu-id="c88ce-189">Pokud používáte prostory úložiště a není zrušte zaškrtnutí políčka **přidejte všechna vhodná úložiště toohello cluster**, Windows hello virtuální disky odpojí během procesu clusteringu hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-189">If you are using Storage Spaces and do not uncheck **Add all eligible storage toohello cluster**, Windows detaches hello virtual disks during hello clustering process.</span></span> <span data-ttu-id="c88ce-190">V důsledku toho se v Správce disků nebo v Průzkumníku nezobrazí, dokud hello prostory úložiště jsou odebrány z hello clusteru a znovu připojit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c88ce-190">As a result, they do not appear in Disk Manager or Explorer until hello storage spaces are removed from hello cluster and reattached using PowerShell.</span></span> <span data-ttu-id="c88ce-191">Prostory úložiště skupiny více disků ve fondech toostorage.</span><span class="sxs-lookup"><span data-stu-id="c88ce-191">Storage Spaces groups multiple disks in toostorage pools.</span></span> <span data-ttu-id="c88ce-192">Další informace najdete v tématu [prostory úložiště](https://technet.microsoft.com/library/hh831739).</span><span class="sxs-lookup"><span data-stu-id="c88ce-192">For more information, see [Storage Spaces](https://technet.microsoft.com/library/hh831739).</span></span>

1. <span data-ttu-id="c88ce-193">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-193">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-194">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-194">Click **Finish**.</span></span>

   <span data-ttu-id="c88ce-195">Správce clusteru převzetí služeb při selhání ukazuje, že se nový uzel clusteru a seznamů v hello **uzly** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-195">Failover Cluster Manager shows that your cluster has a new node and lists it in hello **Nodes** container.</span></span>

10. <span data-ttu-id="c88ce-196">Odhlaste se z relace vzdálené plochy hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-196">Log out of hello remote desktop session.</span></span>

### <a name="add-a-cluster-quorum-file-share"></a><span data-ttu-id="c88ce-197">Přidejte sdílenou složku kvora clusteru</span><span class="sxs-lookup"><span data-stu-id="c88ce-197">Add a cluster quorum file share</span></span>

<span data-ttu-id="c88ce-198">V tomto příkladu používá clusteru se systémem Windows hello toocreate sdílenou složku soubor kvorum clusteru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-198">In this example hello Windows cluster uses a file share toocreate a cluster quorum.</span></span> <span data-ttu-id="c88ce-199">Tento kurz používá většina uzlů a sdílených sdílené složky kvora.</span><span class="sxs-lookup"><span data-stu-id="c88ce-199">This tutorial uses a Node and File Share Majority quorum.</span></span> <span data-ttu-id="c88ce-200">Další informace najdete v tématu [Principy konfigurací kvora v clusteru s podporou převzetí služeb při selhání](http://technet.microsoft.com/library/cc731739.aspx).</span><span class="sxs-lookup"><span data-stu-id="c88ce-200">For more information, see [Understanding Quorum Configurations in a Failover Cluster](http://technet.microsoft.com/library/cc731739.aspx).</span></span>

1. <span data-ttu-id="c88ce-201">Připojte toohello souboru sdílenou složku určující členský server s relaci vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c88ce-201">Connect toohello file share witness member server with a remote desktop session.</span></span>

1. <span data-ttu-id="c88ce-202">Na **správce serveru**, klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-202">On **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="c88ce-203">Otevřete **Správa počítače**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-203">Open **Computer Management**.</span></span>

1. <span data-ttu-id="c88ce-204">Klikněte na tlačítko **sdílených složek**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-204">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="c88ce-205">Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-205">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="c88ce-207">Použití **Průvodce vytvořením sdílené složky** toocreate sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c88ce-207">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="c88ce-208">Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-208">On **Folder Path**, click **Browse** and locate or create a path for hello shared folder.</span></span> <span data-ttu-id="c88ce-209">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-209">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-210">V **název, popis a nastavení** ověřte název sdílené složky hello a cestu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-210">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="c88ce-211">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-211">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-212">Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-212">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="c88ce-213">Klikněte na tlačítko **vlastní...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-213">Click **Custom...**.</span></span>

1. <span data-ttu-id="c88ce-214">Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-214">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="c88ce-215">Zajistěte, aby hello účet používaný toocreate hello clusteru má plnou kontrolu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-215">Make sure that hello account used toocreate hello cluster has full control.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. <span data-ttu-id="c88ce-217">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-217">Click **OK**.</span></span>

1. <span data-ttu-id="c88ce-218">V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-218">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="c88ce-219">Klikněte na tlačítko **Dokončit** znovu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-219">Click **Finish** again.</span></span>  

1. <span data-ttu-id="c88ce-220">Odhlaste se ze serveru hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-220">Log out of hello server</span></span>

### <a name="configure-cluster-quorum"></a><span data-ttu-id="c88ce-221">Konfigurace kvora clusteru</span><span class="sxs-lookup"><span data-stu-id="c88ce-221">Configure cluster quorum</span></span>

<span data-ttu-id="c88ce-222">Dále nastavte kvorum clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-222">Next, set hello cluster quorum.</span></span>

1. <span data-ttu-id="c88ce-223">Připojte toohello prvním uzlu clusteru pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="c88ce-223">Connect toohello first cluster node with remote desktop.</span></span>

1. <span data-ttu-id="c88ce-224">V **Správce clusteru převzetí služeb při selhání**, klikněte pravým tlačítkem na hello clusteru, přejděte příliš**další akce**a klikněte na tlačítko **konfigurovat nastavení kvora clusteru...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-224">In **Failover Cluster Manager**, right-click hello cluster, point too**More Actions**, and click **Configure Cluster Quorum Settings...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. <span data-ttu-id="c88ce-226">V **Průvodce konfigurací kvora clusteru**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-226">In **Configure Cluster Quorum Wizard**, click **Next**.</span></span>

1. <span data-ttu-id="c88ce-227">V **vybrat možnosti konfigurace kvora**, zvolte **vybrat určující disk kvora hello**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-227">In **Select Quorum Configuration Option**, choose **Select hello quorum witness**, and click **Next**.</span></span>

1. <span data-ttu-id="c88ce-228">Na **vybrat určující disk kvora**, klikněte na tlačítko **nakonfigurovat určující sdílenou složku**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-228">On **Select Quorum Witness**, click **Configure a file share witness**.</span></span>

   >[!TIP]
   ><span data-ttu-id="c88ce-229">Windows Server 2016 podporuje určujícího cloudu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-229">Windows Server 2016 supports a cloud witness.</span></span> <span data-ttu-id="c88ce-230">Pokud si zvolíte tento typ určující sdílené složky, není nutné soubor sdílet s kopií clusteru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-230">If you choose this type of witness, you do not need a file share witness.</span></span> <span data-ttu-id="c88ce-231">Další informace najdete v tématu [nasazení cloudu určující pro Cluster s podporou převzetí služeb při selhání](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="c88ce-231">For more information, see [Deploy a cloud witness for a Failover Cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span> <span data-ttu-id="c88ce-232">Tento kurz používá určující sdílenou složku souboru, který není podporován ve starších operačních systémech.</span><span class="sxs-lookup"><span data-stu-id="c88ce-232">This tutorial uses a file share witness, which is supported by previous operating systems.</span></span>

1. <span data-ttu-id="c88ce-233">Na **nakonfigurovat určující sdílenou složku**, zadejte hello cestu pro sdílenou složku hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c88ce-233">On **Configure File Share Witness**, type hello path for hello share you created.</span></span> <span data-ttu-id="c88ce-234">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-234">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-235">Ověřte nastavení hello na **potvrzení**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-235">Verify hello settings on **Confirmation**.</span></span> <span data-ttu-id="c88ce-236">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-236">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-237">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-237">Click **Finish**.</span></span>

<span data-ttu-id="c88ce-238">základní prostředky clusteru Hello jsou nakonfigurovány s určující sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c88ce-238">hello cluster core resources are configured with a file share witness.</span></span>

## <a name="enable-availability-groups"></a><span data-ttu-id="c88ce-239">Povolte skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c88ce-239">Enable Availability Groups</span></span>

<span data-ttu-id="c88ce-240">Dál povolte hello **skupiny dostupnosti AlwaysOn** funkce.</span><span class="sxs-lookup"><span data-stu-id="c88ce-240">Next, enable hello **AlwaysOn Availability Groups** feature.</span></span> <span data-ttu-id="c88ce-241">Proveďte tyto kroky na obou serverech SQL.</span><span class="sxs-lookup"><span data-stu-id="c88ce-241">Do these steps on both SQL Servers.</span></span>

1. <span data-ttu-id="c88ce-242">Z hello **spustit** obrazovky, spusťte **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-242">From hello **Start** screen, launch **SQL Server Configuration Manager**.</span></span>
2. <span data-ttu-id="c88ce-243">Ve stromu hello prohlížeče, klikněte na tlačítko **služby SQL Server**, klikněte pravým tlačítkem hello **serveru SQL (MSSQLSERVER)** služby a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-243">In hello browser tree, click **SQL Server Services**, then right-click hello **SQL Server (MSSQLSERVER)** service and click **Properties**.</span></span>
3. <span data-ttu-id="c88ce-244">Klikněte na tlačítko hello **vysoké dostupnosti AlwaysOn** a potom vyberte **povolte skupiny dostupnosti AlwaysOn**, a to takto:</span><span class="sxs-lookup"><span data-stu-id="c88ce-244">Click hello **AlwaysOn High Availability** tab, then select **Enable AlwaysOn Availability Groups**, as follows:</span></span>

    ![Povolte skupiny dostupnosti AlwaysOn](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. <span data-ttu-id="c88ce-246">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-246">Click **Apply**.</span></span> <span data-ttu-id="c88ce-247">Klikněte na tlačítko **OK** v místním dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-247">Click **OK** in hello pop-up dialog.</span></span>

5. <span data-ttu-id="c88ce-248">Restartujte službu SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-248">Restart hello SQL Server service.</span></span>

<span data-ttu-id="c88ce-249">Opakujte tyto kroky na hello jiný Server SQL.</span><span class="sxs-lookup"><span data-stu-id="c88ce-249">Repeat these steps on hello other SQL Server.</span></span>

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for hello database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for hello instance of SQL Server that is used toosynchronize hello database replicas in hello Availability Groups on that instance.

On both SQL Servers, open hello firewall for hello TCP port for hello database mirroring endpoint.

1. On hello first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In hello left pane, select **Inbound Rules**. On hello right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For hello port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In hello **Action** page, keep **Allow hello connection** selected and click **Next**.
6. In hello **Profile** page, accept hello default settings and click **Next**.
7. In hello **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in hello **Name** text box, then click **Finish**.

Repeat these steps on hello second SQL Server.
-------------------------->

## <a name="create-a-database-on-hello-first-sql-server"></a><span data-ttu-id="c88ce-250">Vytvořit databázi na hello první systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-250">Create a database on hello first SQL Server</span></span>

1. <span data-ttu-id="c88ce-251">Spuštění hello RDP souboru toohello prvního serveru SQL s doménou účet, který je členem skupiny sysadmin, pevné role serveru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-251">Launch hello RDP file toohello first SQL Server with a domain account that is a member of sysadmin fixed server role.</span></span>
1. <span data-ttu-id="c88ce-252">Otevřete aplikaci SQL Server Management Studio a připojte toohello první systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-252">Open SQL Server Management Studio and connect toohello first SQL Server.</span></span>
7. <span data-ttu-id="c88ce-253">V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze** a klikněte na tlačítko **novou databázi**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-253">In **Object Explorer**, right-click **Databases** and click **New Database**.</span></span>
8. <span data-ttu-id="c88ce-254">V **název databáze**, typ **MyDB1**, pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-254">In **Database name**, type **MyDB1**, then click **OK**.</span></span>

### <span data-ttu-id="c88ce-255"><a name="backupshare"></a>Vytvoří sdílenou složku zálohování</span><span class="sxs-lookup"><span data-stu-id="c88ce-255"><a name="backupshare"></a> Create a backup share</span></span>

1. <span data-ttu-id="c88ce-256">Na hello první SQL Server v **správce serveru**, klikněte na tlačítko **nástroje**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-256">On hello first SQL Server in **Server Manager**, click **Tools**.</span></span> <span data-ttu-id="c88ce-257">Otevřete **Správa počítače**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-257">Open **Computer Management**.</span></span>

1. <span data-ttu-id="c88ce-258">Klikněte na tlačítko **sdílených složek**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-258">Click **Shared Folders**.</span></span>

1. <span data-ttu-id="c88ce-259">Klikněte pravým tlačítkem na **sdílené složky**a klikněte na tlačítko **novou sdílenou složku...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-259">Right-click **Shares**, and click **New Share...**.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   <span data-ttu-id="c88ce-261">Použití **Průvodce vytvořením sdílené složky** toocreate sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="c88ce-261">Use **Create a Shared Folder Wizard** toocreate a share.</span></span>

1. <span data-ttu-id="c88ce-262">Na **cesta ke složce**, klikněte na tlačítko **Procházet** a najít nebo vytvořit cestu pro sdílenou složku hello databáze zálohování.</span><span class="sxs-lookup"><span data-stu-id="c88ce-262">On **Folder Path**, click **Browse** and locate or create a path for hello database backup shared folder.</span></span> <span data-ttu-id="c88ce-263">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-263">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-264">V **název, popis a nastavení** ověřte název sdílené složky hello a cestu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-264">In **Name, Description, and Settings** verify hello share name and path.</span></span> <span data-ttu-id="c88ce-265">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-265">Click **Next**.</span></span>

1. <span data-ttu-id="c88ce-266">Na **sdílené složky oprávnění** nastavit **přizpůsobit oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-266">On **Shared Folder Permissions** set **Customize permissions**.</span></span> <span data-ttu-id="c88ce-267">Klikněte na tlačítko **vlastní...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-267">Click **Custom...**.</span></span>

1. <span data-ttu-id="c88ce-268">Na **upravit oprávnění**, klikněte na tlačítko **přidat...** .</span><span class="sxs-lookup"><span data-stu-id="c88ce-268">On **Customize Permissions**, click **Add...**.</span></span>

1. <span data-ttu-id="c88ce-269">Ujistěte se, že hello systému SQL Server a SQL Server Agent účty služby pro oba servery mít plnou kontrolu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-269">Make sure that hello SQL Server and SQL Server Agent service accounts for both servers have full control.</span></span>

   ![Nové sdílené složky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. <span data-ttu-id="c88ce-271">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-271">Click **OK**.</span></span>

1. <span data-ttu-id="c88ce-272">V **sdílené složky oprávnění**, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-272">In **Shared Folder Permissions**, click **Finish**.</span></span> <span data-ttu-id="c88ce-273">Klikněte na tlačítko **Dokončit** znovu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-273">Click **Finish** again.</span></span>  

### <a name="take-a-full-backup-of-hello-database"></a><span data-ttu-id="c88ce-274">Proveďte úplnou zálohu databáze hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-274">Take a full backup of hello database</span></span>

<span data-ttu-id="c88ce-275">Je nutné tooback řetězem hello nové databáze tooinitialize hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-275">You need tooback up hello new database tooinitialize hello log chain.</span></span> <span data-ttu-id="c88ce-276">Pokud neprovedete zálohování hello nové databáze, nemůže být součástí skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c88ce-276">If you do not take a backup of hello new database, it cannot be included in an Availability Group.</span></span>

1. <span data-ttu-id="c88ce-277">V **Průzkumník objektů**, klikněte pravým tlačítkem na databázi hello, přejděte příliš**úlohy...** , klikněte na tlačítko **zálohování**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-277">In **Object Explorer**, right-click hello database, point too**Tasks...**, click **Back Up**.</span></span>

1. <span data-ttu-id="c88ce-278">Klikněte na tlačítko **OK** tootake umístění zálohy úplného zálohování toohello výchozí.</span><span class="sxs-lookup"><span data-stu-id="c88ce-278">Click **OK** tootake a full backup toohello default backup location.</span></span>

## <a name="create-hello-availability-group"></a><span data-ttu-id="c88ce-279">Vytvoření skupiny dostupnosti hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-279">Create hello Availability Group</span></span>
<span data-ttu-id="c88ce-280">Jste nyní připraven tooconfigure, které skupiny dostupnosti pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c88ce-280">You are now ready tooconfigure an Availability Group using hello following steps:</span></span>

* <span data-ttu-id="c88ce-281">Vytvořit databázi na hello první systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-281">Create a database on hello first SQL Server.</span></span>
* <span data-ttu-id="c88ce-282">Proveďte úplné zálohování a zálohování protokolu transakcí databáze hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-282">Take both a full backup and a transaction log backup of hello database</span></span>
* <span data-ttu-id="c88ce-283">Úplné obnovení hello a toohello zálohy protokolu druhé systému SQL Server s hello **NORECOVERY** možnost</span><span class="sxs-lookup"><span data-stu-id="c88ce-283">Restore hello full and log backups toohello second SQL Server with hello **NORECOVERY** option</span></span>
* <span data-ttu-id="c88ce-284">Vytvoření hello skupiny dostupnosti (**AG1**) se synchronním potvrzováním, automatické převzetí služeb při selhání a čitelných místních replikách</span><span class="sxs-lookup"><span data-stu-id="c88ce-284">Create hello Availability Group (**AG1**) with synchronous commit, automatic failover, and readable secondary replicas</span></span>

### <a name="create-hello-availability-group"></a><span data-ttu-id="c88ce-285">Vytvoření skupiny dostupnosti hello:</span><span class="sxs-lookup"><span data-stu-id="c88ce-285">Create hello Availability Group:</span></span>

1. <span data-ttu-id="c88ce-286">V relaci vzdálené plochy toohello první systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-286">On remote desktop session toohello first SQL Server.</span></span> <span data-ttu-id="c88ce-287">V **Průzkumník objektů** v aplikaci SSMS, klikněte pravým tlačítkem na **vysoké dostupnosti AlwaysOn** a klikněte na tlačítko **Průvodce novou skupinou dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-287">In **Object Explorer** in SSMS, right-click **AlwaysOn High Availability** and click **New Availability Group Wizard**.</span></span>

    ![Spusťte Průvodce novou skupinou dostupnosti](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. <span data-ttu-id="c88ce-289">V hello **ÚVOD** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-289">In hello **Introduction** page, click **Next**.</span></span> <span data-ttu-id="c88ce-290">V hello **zadejte název skupiny dostupnosti** stránky, zadejte název hello skupiny dostupnosti, například **AG1**v **název skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-290">In hello **Specify Availability Group Name** page, type a name for hello Availability Group, for example **AG1**, in **Availability group name**.</span></span> <span data-ttu-id="c88ce-291">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-291">Click **Next**.</span></span>

    ![Průvodce novým AG, zadejte název skupiny AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. <span data-ttu-id="c88ce-293">V hello **vyberte databáze** , vyberte svou databázi a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-293">In hello **Select Databases** page, select your database and click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="c88ce-294">databáze Hello splňuje požadavky hello pro skupinu dostupnosti, protože jste provedli aspoň jednu úplnou zálohu na hello určený primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c88ce-294">hello database meets hello prerequisites for an Availability Group because you have taken at least one full backup on hello intended primary replica.</span></span>

   ![Průvodce novým AG, vyberte možnost databáze](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. <span data-ttu-id="c88ce-296">V hello **zadejte repliky** klikněte na tlačítko **přidat repliky**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-296">In hello **Specify Replicas** page, click **Add Replica**.</span></span>

   ![Průvodce novým AG, zadejte repliky](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. <span data-ttu-id="c88ce-298">Hello **připojit tooServer** objeví dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c88ce-298">hello **Connect tooServer** dialog pops up.</span></span> <span data-ttu-id="c88ce-299">Název typu hello druhý server hello v **název serveru**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-299">Type hello name of hello second server in **Server name**.</span></span> <span data-ttu-id="c88ce-300">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-300">Click **Connect**.</span></span>

   <span data-ttu-id="c88ce-301">Zpět v hello **zadejte repliky** stránky, měli byste vidět druhý server hello uvedené v **replik dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-301">Back in hello **Specify Replicas** page, you should now see hello second server listed in **Availability Replicas**.</span></span> <span data-ttu-id="c88ce-302">Konfigurace replik hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c88ce-302">Configure hello replicas as follows.</span></span>

   ![Průvodce novým AG, zadejte repliky (dokončeno)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. <span data-ttu-id="c88ce-304">Klikněte na tlačítko **koncové body** databáze hello toosee koncový bod zrcadlení pro tuto skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c88ce-304">Click **Endpoints** toosee hello database mirroring endpoint for this Availability Group.</span></span> <span data-ttu-id="c88ce-305">Hello použijte stejný port, který jste použili při nastavíte hello [pravidlo brány firewall pro koncové body zrcadlení databáze](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="c88ce-305">Use hello same port that you used when you set hello [firewall rule for database mirroring endpoints](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. <span data-ttu-id="c88ce-307">V hello **vybrat počáteční synchronizaci dat** vyberte **úplné** a zadejte do sdíleného síťového umístění.</span><span class="sxs-lookup"><span data-stu-id="c88ce-307">In hello **Select Initial Data Synchronization** page, select **Full** and specify a shared network location.</span></span> <span data-ttu-id="c88ce-308">Umístění hello použít hello [zálohování sdílené složky, kterou jste vytvořili](#backupshare).</span><span class="sxs-lookup"><span data-stu-id="c88ce-308">For hello location, use hello [backup share that you created](#backupshare).</span></span> <span data-ttu-id="c88ce-309">V příkladu hello byl **\\\\\<první systému SQL Server\>\Backup\**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-309">In hello example it was, **\\\\\<First SQL Server\>\Backup\**.</span></span> <span data-ttu-id="c88ce-310">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-310">Click **Next**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="c88ce-311">Úplná synchronizace trvá úplnou zálohu databáze hello na hello první instance systému SQL Server a obnoví se druhá instance toohello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-311">Full synchronization takes a full backup of hello database on hello first instance of SQL Server and restores it toohello second instance.</span></span> <span data-ttu-id="c88ce-312">Pro velké databáze úplná synchronizace nedoporučuje, protože to může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-312">For large databases, full synchronization is not recommended because it may take a long time.</span></span> <span data-ttu-id="c88ce-313">Tuto chvíli můžete snížit tak, že ručně zálohu databáze hello obnovení její `NO RECOVERY`.</span><span class="sxs-lookup"><span data-stu-id="c88ce-313">You can reduce this time by manually taking a backup of hello database and restoring it with `NO RECOVERY`.</span></span> <span data-ttu-id="c88ce-314">Pokud již obnovení databáze hello s `NO RECOVERY` na hello druhé před konfigurací hello skupiny dostupnosti systému SQL Server, vyberte **připojení pouze**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-314">If hello database is already restored with `NO RECOVERY` on hello second SQL Server before configuring hello Availability Group, choose **Join only**.</span></span> <span data-ttu-id="c88ce-315">Pokud chcete zálohování hello tootake po dokončení konfigurace hello skupiny dostupnosti, vyberte **přeskočit počáteční data synchronizace**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-315">If you want tootake hello backup after configuring hello Availability Group, choose **Skip initial data synchronization**.</span></span>

    ![Průvodce novým AG, vyberte Data počáteční synchronizace](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. <span data-ttu-id="c88ce-317">V hello **ověření** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-317">In hello **Validation** page, click **Next**.</span></span> <span data-ttu-id="c88ce-318">Tato stránka by měl vypadat podobně jako toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="c88ce-318">This page should look similar toohello following image:</span></span>

    ![Průvodce novým AG, ověření](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    ><span data-ttu-id="c88ce-320">Protože jste nenakonfigurovali naslouchací proces skupiny dostupnosti je upozornění pro konfiguraci naslouchacího procesu hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-320">There is a warning for hello listener configuration because you have not configured an Availability Group listener.</span></span> <span data-ttu-id="c88ce-321">Toto upozornění můžete ignorovat, protože na virtuálních počítačích Azure můžete vytvořit naslouchací proces hello po vytvoření pro vyrovnávání zatížení Azure hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-321">You can ignore this warning because on Azure virtual machines you create hello listener after creating hello Azure load balancer.</span></span>

10. <span data-ttu-id="c88ce-322">V hello **Souhrn** klikněte na tlačítko **Dokončit**, potom počkejte, než Průvodce hello nakonfiguruje hello nové skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c88ce-322">In hello **Summary** page, click **Finish**, then wait while hello wizard configures hello new Availability Group.</span></span> <span data-ttu-id="c88ce-323">V hello **průběh** stránky, můžete kliknout na **podrobnosti** tooview hello podrobný průběh.</span><span class="sxs-lookup"><span data-stu-id="c88ce-323">In hello **Progress** page, you can click **More details** tooview hello detailed progress.</span></span> <span data-ttu-id="c88ce-324">Po dokončení Průvodce hello zkontrolovat hello **výsledky** tooverify stránky, která hello skupiny dostupnosti je úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="c88ce-324">Once hello wizard is finished, inspect hello **Results** page tooverify that hello Availability Group is successfully created.</span></span>

     ![Průvodce novým AG, výsledků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. <span data-ttu-id="c88ce-326">Klikněte na tlačítko **Zavřít** tooexit hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="c88ce-326">Click **Close** tooexit hello wizard.</span></span>

### <a name="check-hello-availability-group"></a><span data-ttu-id="c88ce-327">Zkontrolujte hello skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c88ce-327">Check hello Availability Group</span></span>

1. <span data-ttu-id="c88ce-328">V **Průzkumník objektů**, rozbalte položku **vysoké dostupnosti AlwaysOn**, pak rozbalte **skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-328">In **Object Explorer**, expand **AlwaysOn High Availability**, then expand **Availability Groups**.</span></span> <span data-ttu-id="c88ce-329">Teď byste měli vidět text hello nové skupiny dostupnosti v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c88ce-329">You should now see hello new Availability Group in this container.</span></span> <span data-ttu-id="c88ce-330">Klikněte pravým tlačítkem na hello skupiny dostupnosti a klikněte na tlačítko **zobrazit řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-330">Right-click hello Availability Group and click **Show Dashboard**.</span></span>

   ![Řídicí panel zobrazit AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   <span data-ttu-id="c88ce-332">Vaše **řídicí panel AlwaysOn** by měl vypadat podobně jako toothis.</span><span class="sxs-lookup"><span data-stu-id="c88ce-332">Your **AlwaysOn Dashboard** should look similar toothis.</span></span>

   ![Řídicí panel AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   <span data-ttu-id="c88ce-334">Můžete zobrazit hello repliky, hello režim převzetí služeb při selhání každý stav synchronizace repliky a hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-334">You can see hello replicas, hello failover mode of each replica and hello synchronization state.</span></span>

2. <span data-ttu-id="c88ce-335">V **Správce clusteru převzetí služeb při selhání**, klikněte na cluster.</span><span class="sxs-lookup"><span data-stu-id="c88ce-335">In **Failover Cluster Manager**, click your cluster.</span></span> <span data-ttu-id="c88ce-336">Vyberte **role**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-336">Select **Roles**.</span></span> <span data-ttu-id="c88ce-337">Název skupiny dostupnosti Hello, kterého jste je role v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-337">hello Availability Group name you used is a role on hello cluster.</span></span> <span data-ttu-id="c88ce-338">Této skupiny dostupnosti nemá IP adresu pro připojení klientů, protože jste nenakonfigurovali naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c88ce-338">That Availability Group does not have an IP address for client connections, because you did not configure a listener.</span></span> <span data-ttu-id="c88ce-339">Po vytvoření pro vyrovnávání zatížení Azure nakonfigurujete hello naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c88ce-339">You will configure hello listener after you create an Azure load balancer.</span></span>

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > <span data-ttu-id="c88ce-341">Nepokoušejte toofail přes hello skupiny dostupnosti z hello Správce clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c88ce-341">Do not try toofail over hello Availability Group from hello Failover Cluster Manager.</span></span> <span data-ttu-id="c88ce-342">Všechny operace převzetí služeb při selhání by měla provést uvnitř **řídicí panel AlwaysOn** v aplikaci SSMS.</span><span class="sxs-lookup"><span data-stu-id="c88ce-342">All failover operations should be performed from within **AlwaysOn Dashboard** in SSMS.</span></span> <span data-ttu-id="c88ce-343">Další informace najdete v tématu [hello omezení pomocí Správce clusteru převzetí služeb při selhání se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).</span><span class="sxs-lookup"><span data-stu-id="c88ce-343">For more information, see [Restrictions on Using hello Failover Cluster Manager with Availability Groups](https://msdn.microsoft.com/library/ff929171.aspx).</span></span>
    >

<span data-ttu-id="c88ce-344">Nyní máte skupinu dostupnosti s replikami na dvě instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-344">At this point, you have an Availability Group with replicas on two instances of SQL Server.</span></span> <span data-ttu-id="c88ce-345">Hello skupiny dostupnosti můžete přesouvat mezi instancemi.</span><span class="sxs-lookup"><span data-stu-id="c88ce-345">You can move hello Availability Group between instances.</span></span> <span data-ttu-id="c88ce-346">Toohello skupiny dostupnosti nelze ještě připojit, protože nemáte naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="c88ce-346">You cannot connect toohello Availability Group yet because you do not have a listener.</span></span> <span data-ttu-id="c88ce-347">Naslouchací proces hello ve virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c88ce-347">In Azure virtual machines, hello listener requires a load balancer.</span></span> <span data-ttu-id="c88ce-348">dalším krokem Hello je toocreate hello Vyrovnávání zatížení v Azure.</span><span class="sxs-lookup"><span data-stu-id="c88ce-348">hello next step is toocreate hello load balancer in Azure.</span></span>

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a><span data-ttu-id="c88ce-349">Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="c88ce-349">Create an Azure load balancer</span></span>

<span data-ttu-id="c88ce-350">Skupinu dostupnosti SQL Server na virtuálních počítačích Azure, vyžaduje nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c88ce-350">On Azure virtual machines, a SQL Server Availability Group requires a load balancer.</span></span> <span data-ttu-id="c88ce-351">Nástroj pro vyrovnávání zatížení Hello obsahuje hello IP adresu pro naslouchací proces skupiny dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-351">hello load balancer holds hello IP address for hello Availability Group listener.</span></span> <span data-ttu-id="c88ce-352">Tento oddíl shrnuje, jak toocreate hello pro vyrovnávání zátěže v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c88ce-352">This section summarizes how toocreate hello load balancer in hello Azure portal.</span></span>

1. <span data-ttu-id="c88ce-353">V hello portálu Azure, přejděte toohello skupinu prostředků, kde jsou vaše servery SQL a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-353">In hello Azure portal, go toohello resource group where your SQL Servers are and click **+ Add**.</span></span>
2. <span data-ttu-id="c88ce-354">Vyhledejte **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-354">Search for **Load Balancer**.</span></span> <span data-ttu-id="c88ce-355">Vyberte nástroj pro vyrovnávání zatížení hello publikovaný microsoftem.</span><span class="sxs-lookup"><span data-stu-id="c88ce-355">Choose hello load balancer published by Microsoft.</span></span>

   ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  <span data-ttu-id="c88ce-357">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-357">Click **Create**.</span></span>
3. <span data-ttu-id="c88ce-358">Nakonfigurujte následující parametry nástroje pro vyrovnávání zatížení hello hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-358">Configure hello following parameters for hello load balancer.</span></span>

   | <span data-ttu-id="c88ce-359">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-359">Setting</span></span> | <span data-ttu-id="c88ce-360">Pole</span><span class="sxs-lookup"><span data-stu-id="c88ce-360">Field</span></span> |
   | --- | --- |
   | <span data-ttu-id="c88ce-361">**Název**</span><span class="sxs-lookup"><span data-stu-id="c88ce-361">**Name**</span></span> |<span data-ttu-id="c88ce-362">Použít název textu hello Vyrovnávání zatížení, například **sqlLB**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-362">Use a text name for hello load balancer, for example **sqlLB**.</span></span> |
   | <span data-ttu-id="c88ce-363">**Typ**</span><span class="sxs-lookup"><span data-stu-id="c88ce-363">**Type**</span></span> |<span data-ttu-id="c88ce-364">Interní</span><span class="sxs-lookup"><span data-stu-id="c88ce-364">Internal</span></span> |
   | <span data-ttu-id="c88ce-365">**Virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="c88ce-365">**Virtual network**</span></span> |<span data-ttu-id="c88ce-366">Použijte název hello hello virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="c88ce-366">Use hello name of hello Azure virtual network.</span></span> |
   | <span data-ttu-id="c88ce-367">**Podsíť**</span><span class="sxs-lookup"><span data-stu-id="c88ce-367">**Subnet**</span></span> |<span data-ttu-id="c88ce-368">Použijte název hello hello podsítě, která hello virtuální počítač je v.</span><span class="sxs-lookup"><span data-stu-id="c88ce-368">Use hello name of hello subnet that hello virtual machine is in.</span></span>  |
   | <span data-ttu-id="c88ce-369">**Přiřazení IP adresy**</span><span class="sxs-lookup"><span data-stu-id="c88ce-369">**IP address assignment**</span></span> |<span data-ttu-id="c88ce-370">Statická</span><span class="sxs-lookup"><span data-stu-id="c88ce-370">Static</span></span> |
   | <span data-ttu-id="c88ce-371">**IP adresa**</span><span class="sxs-lookup"><span data-stu-id="c88ce-371">**IP address**</span></span> |<span data-ttu-id="c88ce-372">Použijte dostupnou adresu z podsítě.</span><span class="sxs-lookup"><span data-stu-id="c88ce-372">Use an available address from subnet.</span></span> |
   | <span data-ttu-id="c88ce-373">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="c88ce-373">**Subscription**</span></span> |<span data-ttu-id="c88ce-374">Použití hello stejné předplatné jako hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c88ce-374">Use hello same subscription as hello virtual machine.</span></span> |
   | <span data-ttu-id="c88ce-375">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="c88ce-375">**Location**</span></span> |<span data-ttu-id="c88ce-376">Použití hello stejné umístění jako virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-376">Use hello same location as hello virtual machine.</span></span> |

   <span data-ttu-id="c88ce-377">Hello Azure okno portálu by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c88ce-377">hello Azure portal blade should look like this:</span></span>

   ![Vytvořit nástroj pro vyrovnávání zatížení](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. <span data-ttu-id="c88ce-379">Klikněte na tlačítko **vytvořit**, nástroj pro vyrovnávání zatížení toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-379">Click **Create**, toocreate hello load balancer.</span></span>

<span data-ttu-id="c88ce-380">Vyrovnávání zatížení tooconfigure hello potřebujete toocreate fond back-end, test a pravidel Vyrovnávání zatížení hello sady.</span><span class="sxs-lookup"><span data-stu-id="c88ce-380">tooconfigure hello load balancer, you need toocreate a backend pool, a probe, and set hello load balancing rules.</span></span> <span data-ttu-id="c88ce-381">Proveďte v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c88ce-381">Do these in hello Azure portal.</span></span>

### <a name="add-backend-pool"></a><span data-ttu-id="c88ce-382">Přidejte fond back-end</span><span class="sxs-lookup"><span data-stu-id="c88ce-382">Add backend pool</span></span>

1. <span data-ttu-id="c88ce-383">V hello portálu Azure přejděte tooyour skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="c88ce-383">In hello Azure portal, go tooyour availability group.</span></span> <span data-ttu-id="c88ce-384">Může být nutné toorefresh hello zobrazení toosee hello nově vytvořený pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c88ce-384">You might need toorefresh hello view toosee hello newly created load balancer.</span></span>

   ![Najít nástroj pro vyrovnávání zatížení ve skupině prostředků](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. <span data-ttu-id="c88ce-386">Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **back-endové fondy**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-386">Click hello load balancer, click **Backend pools**, and click **+Add**.</span></span> <span data-ttu-id="c88ce-387">Nastavte fond back-end hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c88ce-387">Set hello backend pool as follows:</span></span>

   | <span data-ttu-id="c88ce-388">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-388">Setting</span></span> | <span data-ttu-id="c88ce-389">Popis</span><span class="sxs-lookup"><span data-stu-id="c88ce-389">Description</span></span> | <span data-ttu-id="c88ce-390">Příklad</span><span class="sxs-lookup"><span data-stu-id="c88ce-390">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c88ce-391">**Název**</span><span class="sxs-lookup"><span data-stu-id="c88ce-391">**Name**</span></span> | <span data-ttu-id="c88ce-392">Zadejte název text</span><span class="sxs-lookup"><span data-stu-id="c88ce-392">Type a text name</span></span> | <span data-ttu-id="c88ce-393">SQLLBBE</span><span class="sxs-lookup"><span data-stu-id="c88ce-393">SQLLBBE</span></span>
   | <span data-ttu-id="c88ce-394">**Přidružené k**</span><span class="sxs-lookup"><span data-stu-id="c88ce-394">**Associated to**</span></span> | <span data-ttu-id="c88ce-395">Vybrat ze seznamu</span><span class="sxs-lookup"><span data-stu-id="c88ce-395">Pick from list</span></span> | <span data-ttu-id="c88ce-396">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c88ce-396">Availability set</span></span>
   | <span data-ttu-id="c88ce-397">**Sady dostupnosti.**</span><span class="sxs-lookup"><span data-stu-id="c88ce-397">**Availability set**</span></span> | <span data-ttu-id="c88ce-398">Použijte název sady dostupnosti hello, aby byly virtuální počítače serveru SQL v</span><span class="sxs-lookup"><span data-stu-id="c88ce-398">Use a name of hello availability set that your SQL Server VMs are in</span></span> | <span data-ttu-id="c88ce-399">sqlAvailabilitySet</span><span class="sxs-lookup"><span data-stu-id="c88ce-399">sqlAvailabilitySet</span></span> |
   | <span data-ttu-id="c88ce-400">**Virtual Machines**</span><span class="sxs-lookup"><span data-stu-id="c88ce-400">**Virtual machines**</span></span> |<span data-ttu-id="c88ce-401">Hello dva názvy virtuální počítač Azure SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88ce-401">hello two Azure SQL Server VM names</span></span> | <span data-ttu-id="c88ce-402">SQL Server-0, sqlserver 1</span><span class="sxs-lookup"><span data-stu-id="c88ce-402">sqlserver-0, sqlserver-1</span></span>

1. <span data-ttu-id="c88ce-403">Zadejte název fondu back-end hello hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-403">Type hello name for hello back end pool.</span></span>

1. <span data-ttu-id="c88ce-404">Klikněte na tlačítko **+ přidat virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-404">Click **+ Add a virtual machine**.</span></span>

1. <span data-ttu-id="c88ce-405">Pro sadu dostupnosti hello vyberte tohoto hello, které jsou servery SQL Server ve skupině dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-405">For hello availability set, choose hello availability set that hello SQL Servers are in.</span></span>

1. <span data-ttu-id="c88ce-406">Pro virtuální počítače zahrnují oba hello servery SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88ce-406">For virtual machines, include both of hello SQL Servers.</span></span> <span data-ttu-id="c88ce-407">Nezahrnujte určující server hello sdílené položky.</span><span class="sxs-lookup"><span data-stu-id="c88ce-407">Do not include hello file share witness server.</span></span>

1. <span data-ttu-id="c88ce-408">Klikněte na tlačítko **OK** toocreate hello back-endový fond.</span><span class="sxs-lookup"><span data-stu-id="c88ce-408">Click **OK** toocreate hello backend pool.</span></span>

### <a name="set-hello-probe"></a><span data-ttu-id="c88ce-409">Sada hello testu</span><span class="sxs-lookup"><span data-stu-id="c88ce-409">Set hello probe</span></span>

1. <span data-ttu-id="c88ce-410">Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **testy stavu**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-410">Click hello load balancer, click **Health probes**, and click **+Add**.</span></span>

1. <span data-ttu-id="c88ce-411">Test stavu hello nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="c88ce-411">Set hello health probe as follows:</span></span>

   | <span data-ttu-id="c88ce-412">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-412">Setting</span></span> | <span data-ttu-id="c88ce-413">Popis</span><span class="sxs-lookup"><span data-stu-id="c88ce-413">Description</span></span> | <span data-ttu-id="c88ce-414">Příklad</span><span class="sxs-lookup"><span data-stu-id="c88ce-414">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c88ce-415">**Název**</span><span class="sxs-lookup"><span data-stu-id="c88ce-415">**Name**</span></span> | <span data-ttu-id="c88ce-416">Text</span><span class="sxs-lookup"><span data-stu-id="c88ce-416">Text</span></span> | <span data-ttu-id="c88ce-417">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="c88ce-417">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="c88ce-418">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="c88ce-418">**Protocol**</span></span> | <span data-ttu-id="c88ce-419">Zvolte TCP</span><span class="sxs-lookup"><span data-stu-id="c88ce-419">Choose TCP</span></span> | <span data-ttu-id="c88ce-420">TCP</span><span class="sxs-lookup"><span data-stu-id="c88ce-420">TCP</span></span> |
   | <span data-ttu-id="c88ce-421">**Port**</span><span class="sxs-lookup"><span data-stu-id="c88ce-421">**Port**</span></span> | <span data-ttu-id="c88ce-422">Všechny nepoužívaný port.</span><span class="sxs-lookup"><span data-stu-id="c88ce-422">Any unused port</span></span> | <span data-ttu-id="c88ce-423">59999</span><span class="sxs-lookup"><span data-stu-id="c88ce-423">59999</span></span> |
   | <span data-ttu-id="c88ce-424">**Interval**</span><span class="sxs-lookup"><span data-stu-id="c88ce-424">**Interval**</span></span>  | <span data-ttu-id="c88ce-425">Hello mezi testu pokusů v sekundách</span><span class="sxs-lookup"><span data-stu-id="c88ce-425">hello amount of time between probe attempts in seconds</span></span> |<span data-ttu-id="c88ce-426">5</span><span class="sxs-lookup"><span data-stu-id="c88ce-426">5</span></span> |
   | <span data-ttu-id="c88ce-427">**Prahová hodnota špatného stavu**</span><span class="sxs-lookup"><span data-stu-id="c88ce-427">**Unhealthy threshold**</span></span> | <span data-ttu-id="c88ce-428">Hello počet po sobě jdoucích kontroly chyb, které musí dojít k toobe virtuální počítač považoval za poškozený</span><span class="sxs-lookup"><span data-stu-id="c88ce-428">hello number of consecutive probe failures that must occur for a virtual machine toobe considered unhealthy</span></span>  | <span data-ttu-id="c88ce-429">2</span><span class="sxs-lookup"><span data-stu-id="c88ce-429">2</span></span> |

1. <span data-ttu-id="c88ce-430">Klikněte na tlačítko **OK** test stavu tooset hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-430">Click **OK** tooset hello health probe.</span></span>

### <a name="set-hello-load-balancing-rules"></a><span data-ttu-id="c88ce-431">Nastavit pravidla Vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-431">Set hello load balancing rules</span></span>

1. <span data-ttu-id="c88ce-432">Klikněte na tlačítko hello nástroj pro vyrovnávání zatížení, klikněte na tlačítko **pravidla Vyrovnávání zatížení**a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-432">Click hello load balancer, click **Load balancing rules**, and click **+Add**.</span></span>

1. <span data-ttu-id="c88ce-433">Nastavit pravidla takto Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-433">Set hello load balancing rules as follows.</span></span>
   | <span data-ttu-id="c88ce-434">Nastavení</span><span class="sxs-lookup"><span data-stu-id="c88ce-434">Setting</span></span> | <span data-ttu-id="c88ce-435">Popis</span><span class="sxs-lookup"><span data-stu-id="c88ce-435">Description</span></span> | <span data-ttu-id="c88ce-436">Příklad</span><span class="sxs-lookup"><span data-stu-id="c88ce-436">Example</span></span>
   | --- | --- |---
   | <span data-ttu-id="c88ce-437">**Název**</span><span class="sxs-lookup"><span data-stu-id="c88ce-437">**Name**</span></span> | <span data-ttu-id="c88ce-438">Text</span><span class="sxs-lookup"><span data-stu-id="c88ce-438">Text</span></span> | <span data-ttu-id="c88ce-439">SQLAlwaysOnEndPointListener</span><span class="sxs-lookup"><span data-stu-id="c88ce-439">SQLAlwaysOnEndPointListener</span></span> |
   | <span data-ttu-id="c88ce-440">**Adresa IP front-endu**</span><span class="sxs-lookup"><span data-stu-id="c88ce-440">**Frontend IP address**</span></span> | <span data-ttu-id="c88ce-441">Zvolte adresu</span><span class="sxs-lookup"><span data-stu-id="c88ce-441">Choose an address</span></span> |<span data-ttu-id="c88ce-442">Použijte hello adresu, kterou jste vytvořili při vytváření hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="c88ce-442">Use hello address that you created when you created hello load balancer.</span></span> |
   | <span data-ttu-id="c88ce-443">**Protokol**</span><span class="sxs-lookup"><span data-stu-id="c88ce-443">**Protocol**</span></span> | <span data-ttu-id="c88ce-444">Zvolte TCP</span><span class="sxs-lookup"><span data-stu-id="c88ce-444">Choose TCP</span></span> |<span data-ttu-id="c88ce-445">TCP</span><span class="sxs-lookup"><span data-stu-id="c88ce-445">TCP</span></span> |
   | <span data-ttu-id="c88ce-446">**Port**</span><span class="sxs-lookup"><span data-stu-id="c88ce-446">**Port**</span></span> | <span data-ttu-id="c88ce-447">Použít hello port pro instanci systému SQL Server hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-447">Use hello port for hello SQL Server instance</span></span> | <span data-ttu-id="c88ce-448">1433</span><span class="sxs-lookup"><span data-stu-id="c88ce-448">1433</span></span> |
   | <span data-ttu-id="c88ce-449">**Back-endový Port**</span><span class="sxs-lookup"><span data-stu-id="c88ce-449">**Backend Port**</span></span> | <span data-ttu-id="c88ce-450">Toto pole se nepoužívá při plovoucí IP adresa je nastavena pro přímý server návratový</span><span class="sxs-lookup"><span data-stu-id="c88ce-450">This field is not used when Floating IP is set for direct server return</span></span> | <span data-ttu-id="c88ce-451">1433</span><span class="sxs-lookup"><span data-stu-id="c88ce-451">1433</span></span> |
   | <span data-ttu-id="c88ce-452">**Test**</span><span class="sxs-lookup"><span data-stu-id="c88ce-452">**Probe**</span></span> |<span data-ttu-id="c88ce-453">Zadaný u testu paměti hello název Hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-453">hello name you specified for hello probe</span></span> | <span data-ttu-id="c88ce-454">SQLAlwaysOnEndPointProbe</span><span class="sxs-lookup"><span data-stu-id="c88ce-454">SQLAlwaysOnEndPointProbe</span></span> |
   | <span data-ttu-id="c88ce-455">**Trvalost relace**</span><span class="sxs-lookup"><span data-stu-id="c88ce-455">**Session Persistence**</span></span> | <span data-ttu-id="c88ce-456">Rozevírací seznam</span><span class="sxs-lookup"><span data-stu-id="c88ce-456">Drop down list</span></span> | <span data-ttu-id="c88ce-457">**None**</span><span class="sxs-lookup"><span data-stu-id="c88ce-457">**None**</span></span> |
   | <span data-ttu-id="c88ce-458">**Časový limit nečinnosti**</span><span class="sxs-lookup"><span data-stu-id="c88ce-458">**Idle Timeout**</span></span> | <span data-ttu-id="c88ce-459">Otevřete minut tookeep připojení TCP</span><span class="sxs-lookup"><span data-stu-id="c88ce-459">Minutes tookeep a TCP connection open</span></span> | <span data-ttu-id="c88ce-460">4</span><span class="sxs-lookup"><span data-stu-id="c88ce-460">4</span></span> |
   | <span data-ttu-id="c88ce-461">**Plovoucí IP adresa (přímá odpověď ze serveru)**</span><span class="sxs-lookup"><span data-stu-id="c88ce-461">**Floating IP (direct server return)**</span></span> | |<span data-ttu-id="c88ce-462">Povoleno</span><span class="sxs-lookup"><span data-stu-id="c88ce-462">Enabled</span></span> |

   > [!WARNING]
   > <span data-ttu-id="c88ce-463">Přímá odpověď ze serveru se nastavuje během vytváření.</span><span class="sxs-lookup"><span data-stu-id="c88ce-463">Direct server return is set during creation.</span></span> <span data-ttu-id="c88ce-464">Nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="c88ce-464">It cannot be changed.</span></span>

1. <span data-ttu-id="c88ce-465">Klikněte na tlačítko **OK** pravidla Vyrovnávání zatížení tooset hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-465">Click **OK** tooset hello load balancing rules.</span></span>

## <span data-ttu-id="c88ce-466"><a name="configure-listener"></a>Konfigurace naslouchací proces hello</span><span class="sxs-lookup"><span data-stu-id="c88ce-466"><a name="configure-listener"></a> Configure hello listener</span></span>

<span data-ttu-id="c88ce-467">Další toodo věc Hello je tooconfigure naslouchací proces skupiny dostupnosti v clusteru převzetí služeb při selhání hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-467">hello next thing toodo is tooconfigure an Availability Group listener on hello failover cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c88ce-468">Tento kurz ukazuje, jak toocreate jeden naslouchací proces - s jeden ILB IP adres.</span><span class="sxs-lookup"><span data-stu-id="c88ce-468">This tutorial shows how toocreate a single listener - with one ILB IP address.</span></span> <span data-ttu-id="c88ce-469">toocreate jeden nebo více naslouchací procesy pomocí jednoho nebo více IP adres, najdete v části [skupiny dostupnosti vytvořit naslouchací proces a nástroj pro vyrovnávání zatížení | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c88ce-469">toocreate one or more listeners using one or more IP addresses, see [Create Availability Group listener and load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a><span data-ttu-id="c88ce-470">Port naslouchacího procesu sady</span><span class="sxs-lookup"><span data-stu-id="c88ce-470">Set listener port</span></span>

<span data-ttu-id="c88ce-471">V systému SQL Server Management Studio nastavte hello port naslouchacího procesu.</span><span class="sxs-lookup"><span data-stu-id="c88ce-471">In SQL Server Management Studio, set hello listener port.</span></span>

1. <span data-ttu-id="c88ce-472">Spusťte SQL Server Management Studio a připojte toohello primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c88ce-472">Launch SQL Server Management Studio and connect toohello primary replica.</span></span>

1. <span data-ttu-id="c88ce-473">Přejděte příliš**vysoké dostupnosti AlwaysOn** | **skupiny dostupnosti** | **naslouchací procesy skupiny dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-473">Navigate too**AlwaysOn High Availability** | **Availability Groups** | **Availability Group Listeners**.</span></span>

1. <span data-ttu-id="c88ce-474">Teď byste měli vidět název hello naslouchacího procesu, který jste vytvořili ve Správci clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c88ce-474">You should now see hello listener name that you created in Failover Cluster Manager.</span></span> <span data-ttu-id="c88ce-475">Klikněte pravým tlačítkem na název naslouchacího procesu hello a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-475">Right-click hello listener name and click **Properties**.</span></span>

1. <span data-ttu-id="c88ce-476">V hello **Port** zadejte číslo portu hello pro naslouchací proces skupiny dostupnosti hello pomocí hello $EndpointPort jste použili předtím (1433 byla výchozí hello), pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c88ce-476">In hello **Port** box, specify hello port number for hello Availability Group listener by using hello $EndpointPort you used earlier (1433 was hello default), then click **OK**.</span></span>

<span data-ttu-id="c88ce-477">Nyní máte skupinu dostupnosti SQL Server na virtuálních počítačích Azure spuštěna v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c88ce-477">You now have a SQL Server Availability Group in Azure virtual machines running in Resource Manager mode.</span></span>

## <a name="test-connection-toolistener"></a><span data-ttu-id="c88ce-478">Testovací připojení toolistener</span><span class="sxs-lookup"><span data-stu-id="c88ce-478">Test connection toolistener</span></span>

<span data-ttu-id="c88ce-479">tootest hello připojení:</span><span class="sxs-lookup"><span data-stu-id="c88ce-479">tootest hello connection:</span></span>

1. <span data-ttu-id="c88ce-480">RDP tooa systému SQL Server, který je v hello stejné virtuální síti však není vlastní hello repliky.</span><span class="sxs-lookup"><span data-stu-id="c88ce-480">RDP tooa SQL Server that is in hello same virtual network, but does not own hello replica.</span></span> <span data-ttu-id="c88ce-481">Můžete použít jiný SQL Server v clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="c88ce-481">You can use hello other SQL Server in hello cluster.</span></span>

1. <span data-ttu-id="c88ce-482">Použití **sqlcmd** nástroj tootest hello připojení.</span><span class="sxs-lookup"><span data-stu-id="c88ce-482">Use **sqlcmd** utility tootest hello connection.</span></span> <span data-ttu-id="c88ce-483">Například vytvoří hello následující skript **sqlcmd** připojení toohello primární repliky prostřednictvím hello naslouchací proces s ověřováním systému Windows:</span><span class="sxs-lookup"><span data-stu-id="c88ce-483">For example, hello following script establishes a **sqlcmd** connection toohello primary replica through hello listener with Windows authentication:</span></span>

    ```
    sqlcmd -S <listenerName> -E
    ```

    <span data-ttu-id="c88ce-484">Pokud naslouchací proces hello používá port jiný než hello výchozí port (1433), zadejte hello port v hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c88ce-484">If hello listener is using a port other than hello default port (1433), specify hello port in hello connection string.</span></span> <span data-ttu-id="c88ce-485">Například hello následujícího příkazu sqlcmd připojuje tooa naslouchání na portu 1435:</span><span class="sxs-lookup"><span data-stu-id="c88ce-485">For example, hello following sqlcmd command connects tooa listener at port 1435:</span></span>

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

<span data-ttu-id="c88ce-486">Hello SQLCMD připojení automaticky připojí toowhichever instanci systému SQL Server hostitele hello primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c88ce-486">hello SQLCMD connection automatically connects toowhichever instance of SQL Server hosts hello primary replica.</span></span>

> [!TIP]
> <span data-ttu-id="c88ce-487">Ujistěte se, že je otevřen v bráně firewall hello obou serverů SQL hello port, který zadáte.</span><span class="sxs-lookup"><span data-stu-id="c88ce-487">Make sure that hello port you specify is open on hello firewall of both SQL Servers.</span></span> <span data-ttu-id="c88ce-488">Oba servery vyžadují příchozí pravidlo pro hello port TCP, který používáte.</span><span class="sxs-lookup"><span data-stu-id="c88ce-488">Both servers require an inbound rule for hello TCP port that you use.</span></span> <span data-ttu-id="c88ce-489">Další informace najdete v tématu [přidat nebo upravit pravidlo brány Firewall](http://technet.microsoft.com/library/cc753558.aspx).</span><span class="sxs-lookup"><span data-stu-id="c88ce-489">For more information, see [Add or Edit Firewall Rule](http://technet.microsoft.com/library/cc753558.aspx).</span></span>
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by hello way” info, an Important is info users need toocomplete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is hello second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note hello format for documenting hello UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult toomaintain. Highlight areas you are referring tooin red.*-->

<!--**No. of steps**: *Make sure hello number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want toogo on.*-->

## <a name="next-steps"></a><span data-ttu-id="c88ce-490">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c88ce-490">Next steps</span></span>

- <span data-ttu-id="c88ce-491">[Přidat k IP adresu tooa pro vyrovnávání zatížení pro skupinu dostupnosti druhý](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span><span class="sxs-lookup"><span data-stu-id="c88ce-491">[Add an IP address tooa load balancer for a second availability group](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).</span></span>
