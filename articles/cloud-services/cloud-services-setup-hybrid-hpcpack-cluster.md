---
title: "Nastavení clusteru HPC Pack hybridní v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit malá, hybridní s vysokým výkonem (HPC) clusteru pomocí sady Microsoft HPC Pack a Azure"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: f6dc9657e64160be1e68a7356863b53131e9b3c3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a><span data-ttu-id="25a63-103">Nastavení hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru pomocí sady Microsoft HPC Pack a na vyžádání Azure výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="25a63-103">Set up a hybrid high performance computing (HPC) cluster with Microsoft HPC Pack and on-demand Azure compute nodes</span></span>
<span data-ttu-id="25a63-104">Nastavit malé, hybridní vysokovýkonného výpočetního prostředí (HPC) clusteru pomocí Microsoft HPC Pack 2012 R2 a Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-104">Use Microsoft HPC Pack 2012 R2 and Azure to set up a small, hybrid high performance computing (HPC) cluster.</span></span> <span data-ttu-id="25a63-105">Cluster, uvidíte v tomto článku se skládá z hlavního uzlu místními HPC Pack a některé výpočetní uzly, které nasazujete na vyžádání v Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="25a63-105">The cluster shown in this article consists of an on-premises HPC Pack head node and some compute nodes you deploy on-demand in an Azure cloud service.</span></span> <span data-ttu-id="25a63-106">Když pak spustíte výpočetní úlohy v hybridní clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-106">You can then run compute jobs on the hybrid cluster.</span></span>

![Hybridní clusteru HPC][Overview] 

<span data-ttu-id="25a63-108">Tento kurz ukazuje jeden ze způsobů, někdy označuje jako "shluků do cloudu," clusteru se použije ke spuštění aplikace náročné na výpočetní prostředky Azure, škálovatelnou a na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="25a63-108">This tutorial shows one approach, sometimes called cluster "burst to the cloud," to use scalable, on-demand Azure resources to run compute-intensive applications.</span></span>

<span data-ttu-id="25a63-109">Tento kurz předpokládá žádné předchozí zkušenosti s výpočetní clustery nebo HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="25a63-109">This tutorial assumes no prior experience with compute clusters or HPC Pack.</span></span> <span data-ttu-id="25a63-110">Je určena pouze k vám pomohou při nasazení hybridního výpočetního clusteru rychle pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="25a63-110">It is intended only to help you deploy a hybrid compute cluster quickly for demonstration purposes.</span></span> <span data-ttu-id="25a63-111">Požadavky a kroky k nasazení clusteru HPC Pack hybridní větší škálované v produkčním prostředí, nebo použít HPC Pack 2016, najdete v článku [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="25a63-111">For considerations and steps to deploy a hybrid HPC Pack cluster at greater scale in a production environment, or to use HPC Pack 2016, see the [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span> <span data-ttu-id="25a63-112">Pro scénáře s HPC Pack, včetně automatizované nasazení clusteru ve virtuálních počítačích Azure, najdete v části [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="25a63-112">For other scenarios with HPC Pack, including automated cluster deployment in Azure virtual machines, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25a63-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25a63-113">Prerequisites</span></span>
* <span data-ttu-id="25a63-114">**Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="25a63-114">**Azure subscription** - If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>
* <span data-ttu-id="25a63-115">**Místní počítač se systémem Windows Server 2012 R2 nebo Windows Server 2012** -použít tento počítač jako hlavního uzlu clusteru prostředí HPC.</span><span class="sxs-lookup"><span data-stu-id="25a63-115">**An on-premises computer running Windows Server 2012 R2 or Windows Server 2012** - Use this computer as the head node of the HPC cluster.</span></span> <span data-ttu-id="25a63-116">Pokud už používáte Windows Server, můžete stáhnout a nainstalovat [zkušební verze](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span><span class="sxs-lookup"><span data-stu-id="25a63-116">If you aren't already running Windows Server, you can download and install an [evaluation version](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).</span></span>
  
  * <span data-ttu-id="25a63-117">Počítač musí být připojen k doméně služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25a63-117">The computer must be joined to an Active Directory domain.</span></span> <span data-ttu-id="25a63-118">Pro účely testování můžete počítač hlavního uzlu nakonfigurovat jako řadič domény.</span><span class="sxs-lookup"><span data-stu-id="25a63-118">For test purposes, you can configure the head node computer as a domain controller.</span></span> <span data-ttu-id="25a63-119">Chcete-li přidat roli serveru Active Directory Domain Services a hlavního uzlu počítač jako řadič domény povýšit, najdete v dokumentaci k systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="25a63-119">To add the Active Directory Domain Services server role and promote the head node computer as a domain controller, see the documentation for Windows Server.</span></span>
  * <span data-ttu-id="25a63-120">Pro podporu HPC Pack, musí být nainstalován operační systém v jednom z těchto jazyků: angličtina, japonština nebo čínština (zjednodušená).</span><span class="sxs-lookup"><span data-stu-id="25a63-120">To support HPC Pack, the operating system must be installed in one of these languages: English, Japanese, or Chinese (Simplified).</span></span>
  * <span data-ttu-id="25a63-121">Ověřte, že se nainstalují důležité a kritické aktualizace.</span><span class="sxs-lookup"><span data-stu-id="25a63-121">Verify that important and critical updates are installed.</span></span>
* <span data-ttu-id="25a63-122">**HPC Pack 2012 R2** - [Stáhnout](http://go.microsoft.com/fwlink/p/?linkid=328024) instalaci balíčku na nejnovější verzi zdarma a zkopírujte soubory do počítače hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="25a63-122">**HPC Pack 2012 R2** - [Download](http://go.microsoft.com/fwlink/p/?linkid=328024) the installation package for the latest version free of charge and copy the files to the head node computer.</span></span> <span data-ttu-id="25a63-123">Zvolte instalační soubory ve stejném jazyce jako vaše instalace systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="25a63-123">Choose installation files in the same language as your installation of Windows Server.</span></span>

    >[!NOTE]
    > <span data-ttu-id="25a63-124">Pokud chcete používat HPC Pack 2016 místo HPC Pack 2012 R2, je potřeba další konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="25a63-124">If you want to use HPC Pack 2016 instead of HPC Pack 2012 R2, additional configuration is needed.</span></span> <span data-ttu-id="25a63-125">Najdete v článku [podrobné pokyny](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="25a63-125">See the [detailed guidance](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
    > 
* <span data-ttu-id="25a63-126">**Účet domény** -tento účet musí být nakonfigurované oprávnění místního správce hlavního uzlu instalace sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="25a63-126">**Domain account** - This account must be configured with local Administrator permissions on the head node to install HPC Pack.</span></span>
* <span data-ttu-id="25a63-127">**Připojení TCP na portu 443** z hlavního uzlu do Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-127">**TCP connectivity on port 443** from the head node to Azure.</span></span>

## <a name="install-hpc-pack-on-the-head-node"></a><span data-ttu-id="25a63-128">Instalace sady HPC Pack hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="25a63-128">Install HPC Pack on the head node</span></span>
<span data-ttu-id="25a63-129">První instalaci sady Microsoft HPC Pack v místní počítače se spuštěným systémem Windows Server.</span><span class="sxs-lookup"><span data-stu-id="25a63-129">You first install Microsoft HPC Pack on your on-premises computer running Windows Server.</span></span> <span data-ttu-id="25a63-130">Tento počítač se změní na hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-130">This computer becomes the head node of the cluster.</span></span>

1. <span data-ttu-id="25a63-131">Přihlaste se k hlavnímu uzlu pomocí účtu domény, který má oprávnění místního správce.</span><span class="sxs-lookup"><span data-stu-id="25a63-131">Log on to the head node by using a domain account that has local Administrator permissions.</span></span>

2. <span data-ttu-id="25a63-132">Spusťte Průvodce instalací HPC Pack spuštěním Setup.exe z instalačních souborů HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="25a63-132">Start the HPC Pack Installation Wizard by running Setup.exe from the HPC Pack installation files.</span></span>

3. <span data-ttu-id="25a63-133">Na **instalace HPC Pack 2012 R2** obrazovky, klikněte na tlačítko **nové instalace nebo přidání nových funkcí do existující instalace**.</span><span class="sxs-lookup"><span data-stu-id="25a63-133">On the **HPC Pack 2012 R2 Setup** screen, click **New installation or add new features to an existing installation**.</span></span>

    ![Instalace sady HPC Pack 2012][install_hpc1]

4. <span data-ttu-id="25a63-135">Na **stránku smlouvy uživatele softwaru Microsoft**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-135">On the **Microsoft Software User Agreement page**, click **Next**.</span></span>

5. <span data-ttu-id="25a63-136">Na **vybrat typ instalace** klikněte na tlačítko **vytvoření nového clusteru HPC vytvořením hlavního uzlu**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-136">On the **Select Installation Type** page, click **Create a new HPC cluster by creating a head node**, and then click **Next**.</span></span>

6. <span data-ttu-id="25a63-137">Průvodce spustí několik testy před instalací.</span><span class="sxs-lookup"><span data-stu-id="25a63-137">The wizard runs several pre-installation tests.</span></span> <span data-ttu-id="25a63-138">Klikněte na tlačítko **Další** na **pravidla instalace** Pokud všechny testy byly úspěšné.</span><span class="sxs-lookup"><span data-stu-id="25a63-138">Click **Next** on the **Installation Rules** page if all tests pass.</span></span> <span data-ttu-id="25a63-139">Jinak si prohlédněte informace a proveďte potřebné změny ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="25a63-139">Otherwise, review the information provided and make any necessary changes in your environment.</span></span> <span data-ttu-id="25a63-140">Poté znovu spusťte testy nebo v případě potřeby spustit Průvodce instalací znovu.</span><span class="sxs-lookup"><span data-stu-id="25a63-140">Then run the tests again or if necessary start the Installation Wizard again.</span></span>
7. <span data-ttu-id="25a63-141">Na **HPC DB konfigurace** se přesvědčte, že **hlavní uzel** pro všechny databáze HPC je vybrána a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-141">On the **HPC DB Configuration** page, make sure **Head Node** is selected for all HPC databases, and then click **Next**.</span></span> 

    ![Konfigurace databáze][install_hpc4]

8. <span data-ttu-id="25a63-143">Přijměte výchozí nastavení na dalších stránkách průvodce.</span><span class="sxs-lookup"><span data-stu-id="25a63-143">Accept default selections on the remaining pages of the wizard.</span></span> <span data-ttu-id="25a63-144">Na **nainstalovat požadované součásti** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="25a63-144">On the **Install Required Components** page, click **Install**.</span></span>
   
    ![Instalace][install_hpc6]

9. <span data-ttu-id="25a63-146">Po dokončení instalace, zrušte zaškrtnutí políčka **Start Správce clusteru HPC** a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-146">After the installation completes, uncheck **Start HPC Cluster Manager** and then click **Finish**.</span></span> <span data-ttu-id="25a63-147">(V pozdější fázi spusťte Správce clusteru HPC.)</span><span class="sxs-lookup"><span data-stu-id="25a63-147">(You start HPC Cluster Manager in a later step.)</span></span>
   
    ![Dokončit][install_hpc7]

## <a name="prepare-the-azure-subscription"></a><span data-ttu-id="25a63-149">Příprava předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="25a63-149">Prepare the Azure subscription</span></span>
<span data-ttu-id="25a63-150">Proveďte následující kroky v [portál Azure](https://portal.azure.com) s předplatným Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-150">Perform the following steps in the [Azure portal](https://portal.azure.com) with your Azure subscription.</span></span> <span data-ttu-id="25a63-151">Po dokončení těchto kroků, můžete nasadit Azure uzly z hlavního uzlu na místě.</span><span class="sxs-lookup"><span data-stu-id="25a63-151">After completing these steps, you can deploy Azure nodes from the on-premises head node.</span></span> 
  
  > [!NOTE]
  > <span data-ttu-id="25a63-152">Také si poznamenejte ID vašeho předplatného Azure, které budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="25a63-152">Also make a note of your Azure subscription ID, which you need later.</span></span> <span data-ttu-id="25a63-153">Najít ID v **odběry** na portálu.</span><span class="sxs-lookup"><span data-stu-id="25a63-153">Find the ID in **Subscriptions** in the portal.</span></span>
  > 

### <a name="upload-the-default-management-certificate"></a><span data-ttu-id="25a63-154">Odešlete certifikát správy výchozí</span><span class="sxs-lookup"><span data-stu-id="25a63-154">Upload the default management certificate</span></span>
<span data-ttu-id="25a63-155">HPC Pack nainstaluje certifikát podepsaný svým držitelem z hlavního uzlu, nazývá výchozí Microsoft HPC Azure Management certifikát, který nahrajete jako certifikát pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-155">HPC Pack installs a self-signed certificate on the head node, called the Default Microsoft HPC Azure Management certificate, that you can upload as an Azure management certificate.</span></span> <span data-ttu-id="25a63-156">Tento certifikát slouží pouze k testování a testování konceptu nasazení k zabezpečení připojení mezi Azure a hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="25a63-156">This certificate is provided for testing and proof-of-concept deployments to secure the connection between the head node and Azure.</span></span>

1. <span data-ttu-id="25a63-157">Z hlavního uzlu počítače, přihlaste se k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="25a63-157">From the head node computer, sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="25a63-158">Klikněte na tlačítko **odběry** > *your_subscription_name*.</span><span class="sxs-lookup"><span data-stu-id="25a63-158">Click **Subscriptions** > *your_subscription_name*.</span></span>

3. <span data-ttu-id="25a63-159">Klikněte na tlačítko **certifikáty pro správu** > **nahrát**.4.</span><span class="sxs-lookup"><span data-stu-id="25a63-159">Click **Management certificates** > **Upload**.4.</span></span> <span data-ttu-id="25a63-160">Přejděte na hlavního uzlu pro 2012\Bin\hpccert.cer C:\Program Files\Microsoft HPC Pack souboru.</span><span class="sxs-lookup"><span data-stu-id="25a63-160">Browse on the head node for the file C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer.</span></span> <span data-ttu-id="25a63-161">Potom klikněte na **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="25a63-161">Then, click **Upload**.</span></span>

   
<span data-ttu-id="25a63-162">**Výchozí HPC Azure Management** certifikát se zobrazí v seznamu certifikátů pro správu.</span><span class="sxs-lookup"><span data-stu-id="25a63-162">The **Default HPC Azure Management** certificate appears in the list of management certificates.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="25a63-163">Vytvoření cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="25a63-163">Create an Azure cloud service</span></span>
> [!NOTE]
> <span data-ttu-id="25a63-164">Pro nejlepší výkon vytvoření cloudové služby a účet úložiště (v pozdější fázi) ve stejné geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="25a63-164">For best performance, create the cloud service and the storage account (in a later step) in the same geographic region.</span></span>
> 
> 

1. <span data-ttu-id="25a63-165">Na portálu, klikněte na tlačítko **cloudových služeb (klasické)** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="25a63-165">In the portal, click **Cloud services (classic)** > **+Add**.</span></span>

2.  <span data-ttu-id="25a63-166">Zadejte název DNS pro službu, zvolte skupinu prostředků a umístění a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-166">Type a DNS name for the service, choose a resource group and a location, and then click **Create**.</span></span>


### <a name="create-an-azure-storage-account"></a><span data-ttu-id="25a63-167">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="25a63-167">Create an Azure storage account</span></span>
1. <span data-ttu-id="25a63-168">Na portálu, klikněte na tlačítko **účty úložiště (klasické)** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="25a63-168">In the portal, click **Storage accounts (classic)** > **+Add**.</span></span>

2. <span data-ttu-id="25a63-169">Zadejte název pro účet, vyberte **Classic** modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="25a63-169">Type a name for the account, and select the **Classic** deployment model.</span></span>

3. <span data-ttu-id="25a63-170">Vyberte skupinu prostředků a umístění a nechte ostatní nastavení na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="25a63-170">Choose a resource group and a location, and leave other settings at default values.</span></span> <span data-ttu-id="25a63-171">Poté klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-171">Then click **Create**.</span></span>

## <a name="configure-the-head-node"></a><span data-ttu-id="25a63-172">Konfigurace hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="25a63-172">Configure the head node</span></span>
<span data-ttu-id="25a63-173">Pomocí Správce clusteru HPC nasazení uzlů Azure a odesílání úloh, nejprve provést některé požadované kroky konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-173">To use HPC Cluster Manager to deploy Azure nodes and to submit jobs, first perform some required cluster configuration steps.</span></span>

1. <span data-ttu-id="25a63-174">Z hlavního uzlu spusťte Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="25a63-174">On the head node, start HPC Cluster Manager.</span></span> <span data-ttu-id="25a63-175">Pokud **vyberte hlavní uzel** se zobrazí dialogové okno, klikněte na tlačítko **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="25a63-175">If the **Select Head Node** dialog box appears, click **Local Computer**.</span></span> <span data-ttu-id="25a63-176">**Nasazení na seznam úkolů** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="25a63-176">The **Deployment To-do List** appears.</span></span>

2. <span data-ttu-id="25a63-177">V části **požadované úlohy nasazení**, klikněte na tlačítko **konfigurace sítě**.</span><span class="sxs-lookup"><span data-stu-id="25a63-177">Under **Required deployment tasks**, click **Configure your network**.</span></span>
   
    ![Konfigurace sítě][config_hpc2]

3. <span data-ttu-id="25a63-179">V Průvodci konfigurací sítě vyberte **všechny uzly pouze v podnikové síti** (topologie 5).</span><span class="sxs-lookup"><span data-stu-id="25a63-179">In the Network Configuration Wizard, select **All nodes only on an enterprise network** (Topology 5).</span></span> <span data-ttu-id="25a63-180">Tato konfigurace sítě je nejjednodušší pro demonstrační účely.</span><span class="sxs-lookup"><span data-stu-id="25a63-180">This network configuration is the simplest for demonstration purposes.</span></span>
   
    ![Topologie 5][config_hpc3]
   
4. <span data-ttu-id="25a63-182">Klikněte na tlačítko **Další** přijměte výchozí hodnoty na dalších stránkách průvodce.</span><span class="sxs-lookup"><span data-stu-id="25a63-182">Click **Next** to accept default values on the remaining pages of the wizard.</span></span> <span data-ttu-id="25a63-183">Potom na **zkontrolujte** , klikněte na **konfigurace** k dokončení konfigurace sítě.</span><span class="sxs-lookup"><span data-stu-id="25a63-183">Then, on the **Review** tab, click **Configure** to complete the network configuration.</span></span>

5. <span data-ttu-id="25a63-184">V **nasazení na seznam úkolů**, klikněte na tlačítko **zadejte pověření pro instalaci**.</span><span class="sxs-lookup"><span data-stu-id="25a63-184">In the **Deployment To-do List**, click **Provide installation credentials**.</span></span>

6. <span data-ttu-id="25a63-185">V **pověření pro instalaci** dialogovém okně zadejte přihlašovací údaje účtu domény, který jste použili k instalaci sady HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="25a63-185">In the **Installation Credentials** dialog box, type the credentials of the domain account that you used to install HPC Pack.</span></span> <span data-ttu-id="25a63-186">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="25a63-186">Then click **OK**.</span></span> 
   
    ![Pověření pro instalaci][config_hpc6]
   
7. <span data-ttu-id="25a63-188">V **nasazení na seznam úkolů**, klikněte na tlačítko **konfigurace pojmenování nové uzly**.</span><span class="sxs-lookup"><span data-stu-id="25a63-188">In the **Deployment To-do List**, click **Configure the naming of new nodes**.</span></span>

8. <span data-ttu-id="25a63-189">V **zadejte řady pojmenování uzlu** dialogové okno pole, přijměte výchozí názvy řady a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="25a63-189">In the **Specify Node Naming Series** dialog box, accept the default naming series and click **OK**.</span></span> <span data-ttu-id="25a63-190">Dokončení tohoto kroku, i když Azure uzly, které přidáte v tomto kurzu jsou pojmenované automaticky.</span><span class="sxs-lookup"><span data-stu-id="25a63-190">Complete this step even though the Azure nodes you add in this tutorial are named automatically.</span></span>
   
    ![Pojmenování uzlu][config_hpc8]
   
9. <span data-ttu-id="25a63-192">V **nasazení na seznam úkolů**, klikněte na tlačítko **vytvoření šablony uzlu**.</span><span class="sxs-lookup"><span data-stu-id="25a63-192">In the **Deployment To-do List**, click **Create a node template**.</span></span> <span data-ttu-id="25a63-193">Později v tomto kurzu použijete k přidání uzlů Azure do clusteru šablony uzlu.</span><span class="sxs-lookup"><span data-stu-id="25a63-193">Later in the tutorial, you use the node template to add Azure nodes to the cluster.</span></span>

10. <span data-ttu-id="25a63-194">V uzlu Průvodce vytvořením šablony postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25a63-194">In the Create Node Template Wizard, do the following:</span></span>
    
    <span data-ttu-id="25a63-195">a.</span><span class="sxs-lookup"><span data-stu-id="25a63-195">a.</span></span> <span data-ttu-id="25a63-196">Na **výběr typu šablony uzlu** klikněte na tlačítko **šablony uzlu služby Windows Azure**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-196">On the **Choose Node Template Type** page, click **Windows Azure node template**, and then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc10]
    
    <span data-ttu-id="25a63-198">b.</span><span class="sxs-lookup"><span data-stu-id="25a63-198">b.</span></span> <span data-ttu-id="25a63-199">Klikněte na tlačítko **Další** přijměte výchozí název šablony.</span><span class="sxs-lookup"><span data-stu-id="25a63-199">Click **Next** to accept the default template name.</span></span>
    
    <span data-ttu-id="25a63-200">c.</span><span class="sxs-lookup"><span data-stu-id="25a63-200">c.</span></span> <span data-ttu-id="25a63-201">Na **poskytují informace o předplatném** zadejte svoje ID předplatného Azure (k dispozici v informací o vašem účtu Azure).</span><span class="sxs-lookup"><span data-stu-id="25a63-201">On the **Provide Subscription Information** page, enter your Azure subscription ID (available in your Azure account information).</span></span> <span data-ttu-id="25a63-202">Potom v **certifikát pro správu**, vyhledejte **výchozí Microsoft HPC Azure Management.**</span><span class="sxs-lookup"><span data-stu-id="25a63-202">Then, in **Management certificate**, browse for **Default Microsoft HPC Azure Management.**</span></span> <span data-ttu-id="25a63-203">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-203">Then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc12]
    
    <span data-ttu-id="25a63-205">d.</span><span class="sxs-lookup"><span data-stu-id="25a63-205">d.</span></span> <span data-ttu-id="25a63-206">Na **poskytují informace o službě** vyberte cloudové služby a účet úložiště, který jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="25a63-206">On the **Provide Service Information** page, select the cloud service and the storage account that you created in a previous step.</span></span> <span data-ttu-id="25a63-207">Pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-207">Then click **Next**.</span></span>
    
    ![Šablony uzlu][config_hpc13]
    
    <span data-ttu-id="25a63-209">e.</span><span class="sxs-lookup"><span data-stu-id="25a63-209">e.</span></span> <span data-ttu-id="25a63-210">Klikněte na tlačítko **Další** přijměte výchozí hodnoty na dalších stránkách průvodce.</span><span class="sxs-lookup"><span data-stu-id="25a63-210">Click **Next** to accept default values on the remaining pages of the wizard.</span></span> <span data-ttu-id="25a63-211">Potom na **zkontrolujte** , klikněte na **vytvořit** k vytvoření šablony uzlu.</span><span class="sxs-lookup"><span data-stu-id="25a63-211">Then, on the **Review** tab, click **Create** to create the node template.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="25a63-212">Ve výchozím nastavení Azure uzlu šablona obsahuje nastavení pro spuštění (zřídit) a zastavte uzly ručně, pomocí Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="25a63-212">By default, the Azure node template includes settings for you to start (provision) and stop the nodes manually, using HPC Cluster Manager.</span></span> <span data-ttu-id="25a63-213">Volitelně můžete nakonfigurovat plán, který chcete spustit a zastavit uzlů Azure automaticky.</span><span class="sxs-lookup"><span data-stu-id="25a63-213">You can optionally configure a schedule to start and stop the Azure nodes automatically.</span></span>
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a><span data-ttu-id="25a63-214">Přidání uzlů Azure do clusteru</span><span class="sxs-lookup"><span data-stu-id="25a63-214">Add Azure nodes to the cluster</span></span>
<span data-ttu-id="25a63-215">Uzel šablony je teď možné použijte k přidání uzlů Azure do clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-215">Now use the node template to add Azure nodes to the cluster.</span></span> <span data-ttu-id="25a63-216">Přidání uzlu do clusteru ukládá informace o jejich konfiguraci tak, aby bylo možné spustit (zřídit) je kdykoli v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="25a63-216">Adding the nodes to the cluster stores their configuration information so that you can start (provision) them at any time in the cloud service.</span></span> <span data-ttu-id="25a63-217">Vaše předplatné pouze získá účtovat uzlů Azure po instance běží v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="25a63-217">Your subscription only gets charged for Azure nodes after the instances are running in the cloud service.</span></span>

<span data-ttu-id="25a63-218">Postupujte podle těchto kroků přidejte dva uzly malé.</span><span class="sxs-lookup"><span data-stu-id="25a63-218">Follow these steps to add two Small nodes.</span></span>

1. <span data-ttu-id="25a63-219">Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack) > **přidat uzel**.</span><span class="sxs-lookup"><span data-stu-id="25a63-219">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack) > **Add Node**.</span></span>
   
    ![Přidání uzlu][add_node1]

2. <span data-ttu-id="25a63-221">Průvodce přidáním uzlu na **vybrat způsob nasazení** klikněte na tlačítko **systému Windows Azure přidat uzly**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-221">In the Add Node Wizard, on the **Select Deployment Method** page, click **Add Windows Azure nodes**, and then click **Next**.</span></span>
   
    ![Přidání uzlu Azure][add_node1_1]

3. <span data-ttu-id="25a63-223">Na **zadejte nové uzly** vyberte dříve vytvořenou šablonu Azure uzlu (nazývá ve výchozím nastavení **výchozí šablonu AzureNode**).</span><span class="sxs-lookup"><span data-stu-id="25a63-223">On the **Specify New Nodes** page, select the Azure node template you created previously (called by default **Default AzureNode Template**).</span></span> <span data-ttu-id="25a63-224">Pak zadejte **2** uzly velikosti **malé**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25a63-224">Then specify **2** nodes of size **Small**, and then click **Next**.</span></span>
   
    ![Určit uzlů][add_node2]
   
4. <span data-ttu-id="25a63-226">Na **dokončení Průvodce přidáním uzlu** klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-226">On the **Completing the Add Node Wizard** page, click **Finish**.</span></span>
    
     <span data-ttu-id="25a63-227">Dva uzly Azure s názvem **AzureCN 0001** a **AzureCN 0002**, se nyní zobrazí ve Správci clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="25a63-227">Two Azure nodes, named **AzureCN-0001** and **AzureCN-0002**, now appear in HPC Cluster Manager.</span></span> <span data-ttu-id="25a63-228">Jsou obě **není nasazena** stavu.</span><span class="sxs-lookup"><span data-stu-id="25a63-228">Both are in the **Not-Deployed** state.</span></span>
   
    ![Přidání uzlů][add_node3]

## <a name="start-the-azure-nodes"></a><span data-ttu-id="25a63-230">Spuštění uzlů Azure</span><span class="sxs-lookup"><span data-stu-id="25a63-230">Start the Azure nodes</span></span>
<span data-ttu-id="25a63-231">Pokud chcete použít prostředků clusteru ve službě Azure, pomocí Správce clusteru HPC spustíte uzly (zřídit) Azure a jejich převedení do online režimu.</span><span class="sxs-lookup"><span data-stu-id="25a63-231">When you want to use the cluster resources in Azure, use HPC Cluster Manager to start (provision) the Azure nodes and bring them online.</span></span>

1. <span data-ttu-id="25a63-232">Ve Správci clusteru HPC, klikněte na tlačítko **uzlu správy** (nazývá **Správa prostředků** v aktuálních verzích HPC Pack) a vyberte uzly Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-232">In HPC Cluster Manager, click **Node Management** (called **Resource Management** in current versions of HPC Pack), and select the Azure nodes.</span></span>

2. <span data-ttu-id="25a63-233">Klikněte na tlačítko **spustit**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="25a63-233">Click **Start**, and then click **OK**.</span></span>
   
   ![Počáteční uzly][add_node4]
   
    <span data-ttu-id="25a63-235">Uzly přechod do **zřizování** stavu.</span><span class="sxs-lookup"><span data-stu-id="25a63-235">The nodes transition to the **Provisioning** state.</span></span> <span data-ttu-id="25a63-236">Zobrazte zřizování protokolu, který chcete sledovat průběh zřizování.</span><span class="sxs-lookup"><span data-stu-id="25a63-236">View the provisioning log to track the provisioning progress.</span></span>
   
    ![Zřízení uzly][add_node6]

3. <span data-ttu-id="25a63-238">Po několika minutách uzlů Azure zřídit a jsou v **Offline** stavu.</span><span class="sxs-lookup"><span data-stu-id="25a63-238">After a few minutes, the Azure nodes finish provisioning and are in the **Offline** state.</span></span> <span data-ttu-id="25a63-239">V tomto stavu instance role běží, ale ještě nepovoluje úlohy clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-239">In this state, the role instances are running but cannot yet accept cluster jobs.</span></span>

4. <span data-ttu-id="25a63-240">Chcete-li potvrdit, že instance role běží na portálu Azure klikněte na tlačítko **cloudové služby (klasické)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="25a63-240">To confirm that the role instances are running, in the Azure portal, click **Cloud Services (classic)** > *your_cloud_service_name*.</span></span>
   
   <span data-ttu-id="25a63-241">Měli byste vidět dvě **HpcWorkerRole** instancí (uzlů) spuštěných ve službě.</span><span class="sxs-lookup"><span data-stu-id="25a63-241">You should see two **HpcWorkerRole** instances (nodes) running in the service.</span></span> <span data-ttu-id="25a63-242">HPC Pack také automaticky nasadí dvě **HpcProxy** instance (velikost střední) pro zpracování komunikace mezi hlavního uzlu a Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-242">HPC Pack also automatically deploys two **HpcProxy** instances (size Medium) to handle communication between the head node and Azure.</span></span>

   ![Spuštěné instance][view_instances1]

5. <span data-ttu-id="25a63-244">Aby Azure uzly online ke spuštění úloh clusteru vyberte uzly, klikněte pravým tlačítkem a pak klikněte na tlačítko **přepnout do režimu Online**.</span><span class="sxs-lookup"><span data-stu-id="25a63-244">To bring the Azure nodes online to run cluster jobs, select the nodes, right-click, and then click **Bring Online**.</span></span>
   
    ![Offline uzly][add_node7]
   
    <span data-ttu-id="25a63-246">Správce clusteru HPC označuje, že uzly jsou v **Online** stavu.</span><span class="sxs-lookup"><span data-stu-id="25a63-246">HPC Cluster Manager indicates that the nodes are in the **Online** state.</span></span>

## <a name="run-a-command-across-the-cluster"></a><span data-ttu-id="25a63-247">Spuštění příkazu napříč clusterem</span><span class="sxs-lookup"><span data-stu-id="25a63-247">Run a command across the cluster</span></span>
<span data-ttu-id="25a63-248">Pokud chcete zkontrolovat instalace, použití HPC Pack **clusrun** příkaz ke spuštění příkazu nebo aplikace na jeden nebo více uzlech clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-248">To check the installation, use the HPC Pack **clusrun** command to run a command or application on one or more cluster nodes.</span></span> <span data-ttu-id="25a63-249">Jako jednoduchý příklad, použít **clusrun** získat konfiguraci IP uzlů Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-249">As a simple example, use **clusrun** to get the IP configuration of the Azure nodes.</span></span>

1. <span data-ttu-id="25a63-250">Z hlavního uzlu otevřete příkazový řádek jako správce.</span><span class="sxs-lookup"><span data-stu-id="25a63-250">On the head node, open a command prompt as an administrator.</span></span>

2. <span data-ttu-id="25a63-251">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="25a63-251">Type the following command:</span></span>
   
    `clusrun /nodes:azurecn* ipconfig`

3. <span data-ttu-id="25a63-252">Pokud se zobrazí výzva, zadejte heslo správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="25a63-252">If prompted, enter your cluster administrator password.</span></span> <span data-ttu-id="25a63-253">Měli byste vidět výstup příkazu podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="25a63-253">You should see command output similar to the following.</span></span>
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a><span data-ttu-id="25a63-255">Spustit úlohu testu</span><span class="sxs-lookup"><span data-stu-id="25a63-255">Run a test job</span></span>
<span data-ttu-id="25a63-256">Teď odešlete testovací úlohu, která běží na clusteru hybridní.</span><span class="sxs-lookup"><span data-stu-id="25a63-256">Now submit a test job that runs on the hybrid cluster.</span></span> <span data-ttu-id="25a63-257">V tomto příkladu je úloha jednoduché čištění parametrů (typ vnitřně paralelní výpočty).</span><span class="sxs-lookup"><span data-stu-id="25a63-257">This example is a simple parametric sweep job (a type of intrinsically parallel computation).</span></span> <span data-ttu-id="25a63-258">Tento příklad spustí dílčí úkoly, které přidat celé sama se sebou pomocí **nastavit /a** příkaz.</span><span class="sxs-lookup"><span data-stu-id="25a63-258">This example runs subtasks that add an integer to itself by using the **set /a** command.</span></span> <span data-ttu-id="25a63-259">Všechny uzly v clusteru podílet se na dokončení dílčí úkoly pro celá čísla od 1 do 100.</span><span class="sxs-lookup"><span data-stu-id="25a63-259">All the nodes in the cluster contribute to finishing the subtasks for integers from 1 to 100.</span></span>

1. <span data-ttu-id="25a63-260">Ve Správci clusteru HPC, klikněte na tlačítko **úlohy správy** > **novou čištění úlohu oblouku**.</span><span class="sxs-lookup"><span data-stu-id="25a63-260">In HPC Cluster Manager, click **Job Management** > **New Parametric Sweep Job**.</span></span>
   
    ![Nová úloha][test_job1]

2. <span data-ttu-id="25a63-262">V **novou čištění úlohu oblouku** dialogu **příkazového řádku**, typ `set /a *+*` (přepsání výchozího příkazového řádku, který se zobrazí).</span><span class="sxs-lookup"><span data-stu-id="25a63-262">In the **New Parametric Sweep Job** dialog box, in **Command line**, type `set /a *+*` (overwriting the default command line that appears).</span></span> <span data-ttu-id="25a63-263">Ponechte výchozí hodnoty pro zbývající nastavení a potom klikněte na **odeslání** k odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="25a63-263">Leave default values for the remaining settings, and then click **Submit** to submit the job.</span></span>
   
    ![Čištění parametrů][param_sweep1]

3. <span data-ttu-id="25a63-265">Po dokončení úlohy dvakrát klikněte **Moje oblouku – úloha** úlohy.</span><span class="sxs-lookup"><span data-stu-id="25a63-265">When the job is finished, double-click the **My Sweep Task** job.</span></span>

4. <span data-ttu-id="25a63-266">Klikněte na tlačítko **úlohy v zobrazení**a potom klikněte na dílčí úlohy k zobrazení výstupu počítané na stejné úrovni.</span><span class="sxs-lookup"><span data-stu-id="25a63-266">Click **View Tasks**, and then click a subtask to view the calculated output of that subtask.</span></span>
   
    ![Výsledky úlohy][view_job361]

5. <span data-ttu-id="25a63-268">Chcete-li zjistit, který uzel provádí výpočet pro tento dílčí úlohy, klikněte na tlačítko **přidělené uzly**.</span><span class="sxs-lookup"><span data-stu-id="25a63-268">To see which node performed the calculation for that subtask, click **Allocated Nodes**.</span></span> <span data-ttu-id="25a63-269">(Váš cluster může zobrazovat název jiného uzlu.)</span><span class="sxs-lookup"><span data-stu-id="25a63-269">(Your cluster might show a different node name.)</span></span>
   
    ![Výsledky úlohy][view_job362]

## <a name="stop-the-azure-nodes"></a><span data-ttu-id="25a63-271">Zastavit uzlů Azure</span><span class="sxs-lookup"><span data-stu-id="25a63-271">Stop the Azure nodes</span></span>
<span data-ttu-id="25a63-272">Po můžete vyzkoušet na clusteru, zastavte uzlů Azure, aby se zabránilo zbytečným poplatky ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="25a63-272">After you try out the cluster, stop the Azure nodes to avoid unnecessary charges to your account.</span></span> <span data-ttu-id="25a63-273">Tento krok zastaví cloudové služby a odebere instance rolí Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-273">This step stops the cloud service and removes the Azure role instances.</span></span>

1. <span data-ttu-id="25a63-274">V modulu Správce clusteru HPC v **uzlu správy** (nazývá **Správa prostředků** v předchozích verzích HPC Pack), vyberte oba uzly Azure.</span><span class="sxs-lookup"><span data-stu-id="25a63-274">In HPC Cluster Manager, in **Node Management** (called **Resource Management** in previous versions of HPC Pack), select both Azure nodes.</span></span> <span data-ttu-id="25a63-275">Potom klikněte na **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-275">Then, click **Stop**.</span></span>
   
    ![Zastavit uzly][stop_node1]

2. <span data-ttu-id="25a63-277">V **zastavit systému Windows Azure uzly** dialogové okno, klikněte na tlačítko **Zastavit**.</span><span class="sxs-lookup"><span data-stu-id="25a63-277">In the **Stop Windows Azure nodes** dialog box, click **Stop**.</span></span> 
   
3. <span data-ttu-id="25a63-278">Uzly přechod do **zastavení** stavu.</span><span class="sxs-lookup"><span data-stu-id="25a63-278">The nodes transition to the **Stopping** state.</span></span> <span data-ttu-id="25a63-279">Po několika minutách, Správce clusteru HPC ukazuje, že uzly jsou **nasazení není**.</span><span class="sxs-lookup"><span data-stu-id="25a63-279">After a few minutes, HPC Cluster Manager shows that the nodes are **Not-Deployed**.</span></span>
   
    ![Nenasazeno uzly][stop_node4]

4. <span data-ttu-id="25a63-281">Chcete-li potvrďte, že instance role jsou spuštěné v Azure, na portálu Azure klikněte na tlačítko **cloudových služeb (klasické)** > *your_cloud_service_name*.</span><span class="sxs-lookup"><span data-stu-id="25a63-281">To confirm that the role instances are no longer running in Azure, in the Azure portal, click **Cloud services (classic)** > *your_cloud_service_name*.</span></span> <span data-ttu-id="25a63-282">Žádné instance jsou nasazeny v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="25a63-282">No instances are deployed in the production environment.</span></span> 
   
    <span data-ttu-id="25a63-283">Dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="25a63-283">This completes the tutorial.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25a63-284">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25a63-284">Next steps</span></span>
* <span data-ttu-id="25a63-285">Prozkoumat v dokumentaci k [HPC Pack](https://technet.microsoft.com/library/cc514029).</span><span class="sxs-lookup"><span data-stu-id="25a63-285">Explore the documentation for [HPC Pack](https://technet.microsoft.com/library/cc514029).</span></span>
* <span data-ttu-id="25a63-286">Hybridní nasazení clusteru HPC Pack větší Škálované, naleznete v tématu [rozšíření do instance rolí pracovního procesu Azure pomocí sady Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span><span class="sxs-lookup"><span data-stu-id="25a63-286">To set up a hybrid HPC Pack cluster deployment at greater scale, see [Burst to Azure Worker Role Instances with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).</span></span>
* <span data-ttu-id="25a63-287">Další způsoby vytváření clusteru HPC Pack v Azure, včetně pomocí šablon Azure Resource Manageru, najdete v tématu [možnosti clusteru HPC pomocí sady Microsoft HPC Pack v Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="25a63-287">For other ways to create an HPC Pack cluster in Azure, including using Azure Resource Manager templates, see [HPC cluster options with Microsoft HPC Pack in Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
